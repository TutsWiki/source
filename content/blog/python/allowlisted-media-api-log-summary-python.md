---
date: 2026-11-23
linktitle: Allowlisted media API log summaries in Python
title: Build an allowlisted log summary for media API responses in Python
weight: 40
url: /allowlisted-media-api-log-summary-python/
description: Build a small JSON log view that records media counts without copying signed URLs, captions or debug credentials.
keywords:
  - python
  - json
  - api response
  - application logging
tags: [Python]
---

A media API response can be useful to a download worker and unsuitable for a log file at the same time. The worker may need a complete signed CDN URL, while an operator only needs to know whether a parse returned video, images or audio. Printing the whole response joins those two purposes and can copy temporary signatures or unnecessary content into a second storage system.

This tutorial builds a small log summary with Python's standard library. It does not call a network endpoint or download media. All input values below are synthetic fixtures, including the deliberately added debug field. They are not captured production responses or a claim that any API normally returns credentials.

## Define the information the log actually needs

For this example, the permitted output has five fields:

- `api_status`: an integer application status, if present and in the selected range.
- `platform`: a value from a small local allowlist, otherwise `unknown`.
- `videos`, `images` and `audios`: counts of entries in the corresponding lists.

The application status is a value in the JSON body. It is not proof of the HTTP status or of a successful file transfer. Keep transport status and job state as separate, controlled fields in the surrounding application.

An allowlist is helpful because new response properties remain excluded by default. A denylist that removes only today's sensitive keys can start leaking tomorrow's new fields without a code change. Here, URLs, titles, captions, headers, nested platform data and debug properties never enter the output.

## Build a new dictionary instead of editing the response

Save the following as `safe_log_summary.py` and run it with `python3 safe_log_summary.py`. It uses no third-party packages.

```python
import json
from copy import deepcopy

PLATFORMS = {"tiktok", "instagram", "twitter", "threads", "bilibili"}
KINDS = ("videos", "images", "audios")


def log_summary(payload):
    result = {"api_status": None, "platform": "unknown"}
    result.update({kind: 0 for kind in KINDS})
    if not isinstance(payload, dict):
        return result

    status = payload.get("status")
    if type(status) is int and 100 <= status <= 599:
        result["api_status"] = status

    data = payload.get("data")
    if not isinstance(data, dict):
        return result
    media = data.get("media")
    if not isinstance(media, dict):
        media = data

    platform = media.get("platform")
    if isinstance(platform, str) and platform in PLATFORMS:
        result["platform"] = platform
    for kind in KINDS:
        items = media.get(kind)
        if isinstance(items, list):
            result[kind] = len(items)
    return result


fixture = {
    "status": 200,
    "data": {
        "media": {
            "platform": "instagram",
            "videos": [{"url": "https://cdn.example.invalid/a.mp4?sig=DEMO"}],
            "images": [],
            "audios": [],
        },
        "platformData": {"caption": "PRIVATE_TEST_TEXT"},
    },
    "debug": {"Authorization": "Bearer DEMO_NOT_A_REAL_TOKEN"},
}

if __name__ == "__main__":
    original = deepcopy(fixture)
    summary = log_summary(fixture)
    encoded = json.dumps(summary, sort_keys=True)
    assert summary == {"api_status": 200, "platform": "instagram",
                       "videos": 1, "images": 0, "audios": 0}
    assert not any(secret in encoded for secret in
                   ("DEMO", "PRIVATE_TEST_TEXT", "Authorization", "http"))
    assert fixture == original
    assert log_summary({"status": True})["api_status"] is None
    assert log_summary(None)["platform"] == "unknown"
    assert log_summary({"data": {"videos": None}})["videos"] == 0
    assert log_summary({"data": {"platform": "secret.example.invalid"}})["platform"] == "unknown"
    assert log_summary({"data": {"platform": "tiktok", "images": [{}, {}]}})["images"] == 2
    print(encoded)
    print("Eight offline checks passed.")
```

The normalizer recognizes either `data.media` or media lists directly under `data`. These are examples of two supported envelope layouts, not a universal API standard. If your provider uses another structure, change the extraction step to match its documented schema. The five platform names are a local logging policy, not a list of every platform a provider supports.

The output is:

```text
{"api_status": 200, "audios": 0, "images": 0, "platform": "instagram", "videos": 1}
Eight offline checks passed.
```

Notice the exact type check for status. Python treats `True` as an instance of `int`; using `type(status) is int` prevents a JSON boolean from accidentally becoming an application status in the log.

Missing data, null media lists and unfamiliar platform strings produce a small conservative summary. A count is the number of returned entries, not a promise that every entry is a usable download. The summary intentionally does not determine whether parsing, billing or a later media transfer succeeded.

## Preserve the full object for its original job

The function constructs a new dictionary and does not replace, shorten or strip anything inside the input. A download worker can therefore continue using the original media object, including any full signed URL and documented public request headers. Do not feed the summary back into the downloader: it deliberately contains none of the information needed to retrieve a file.

Store the original response only where that application workflow requires it. Choose access controls and retention separately from logs. In particular, avoid adding the raw payload back as an extra field next to this summary; that would undo the boundary the function creates.

## Use the summary at an API integration boundary

Disclosure: I build EasyDown. The [EasyDown API documentation](https://easydown.org/docs/en/api) describes a paid developer API for supported public single-post media URLs, separate from the free browser downloader. The logging pattern here is independent of that service: adapt the input extraction to your own API contract and keep operational logs smaller than the response body.

A caller can combine the returned dictionary with a generated job ID and an internal stage name such as `parsed` or `download_failed`. Use controlled stage values rather than arbitrary exception text, which can itself contain URLs or credentials. A file-transfer failure should remain a transfer failure; the media count does not justify retrying a paid parse automatically.

This is a narrow logging helper, not a complete security filter, response validator or compliance guarantee. It does not sanitize other log messages, tracing tools, proxies or error reporters. The eight checks run locally on fixtures and make no claim about live API availability. This original tutorial was prepared with AI assistance; its included code and stated output were checked locally.
