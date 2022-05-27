---
title: Debug Meeting SDK - Video SDK Docs
hide_title: false
hide_table_of_contents: false
description: This tutorial describes the process of joining and leave video and audio call using javascript.
sidebar_label: Debug Meeting SDK
pagination_label: Debug Meeting SDK
keywords:
  - Debug mode
  - error event
image: img/videosdklive-thumbnail.jpg
sidebar_position: 1
slug: debug-mode
---

# Listen for development and production errors
This event is helpful for development level troubleshooting while integrating SDK. Whenever any invalid configuration provided or server/network errors arise, this event will trigger with a specific error code and message.

This event can ease your development process. We have depicted a specific constant with code and message in the below table.

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

```js
import { VideoSDK } from "@videosdk.live/js-sdk";

meeting.on("error", (data) => {
  const { code, message } = data;

  // Get Constant from SDK which contain value of error Code
  const { INVALID_TOKEN, INVALID_MEETING_ID } = VideoSDK.constants.errors;

  switch (code) {
    case INVALID_TOKEN:
      console.log(`Error is ${message}`);
      break;

    case INVALID_MEETING_ID:
      console.log(`Error is ${message}`);
      break;

    default:
      break;
  }
});
```


| Constant                      | Code | Message                                                                                                               |
| ----------------------------- | ---- | --------------------------------------------------------------------------------------------------------------------- |
| **INVALID_API_KEY**           | 4001 | `apikey` provided in the token is empty or invalid, please verify it on the dashboard.                                |
| **INVALID_TOKEN**             | 4002 | `token` is empty or invalid or might have expired.                                                                    |
| **INVALID_MEETING_ID**        | 4003 | `meetingId` is empty or invalid, please verify it or generate new meetingId using the API.                            |
| **INVALID_PARTICIPANT_ID**    | 4004 | `participantId` is empty or invalid, it shouldn't contain any whitespaces.                                            |
| **DUPLICATE_PARTICIPANT**     | 4005 | Leaving meeting, since this `participantId` joined from another device.                                               |
| **ACCOUNT_DEACTIVATED**       | 4006 | It seems your account is deactivated by VideoSDK for some reason, you can reach out to us at `support@videosdk.live`. |
| **ACCOUNT_DISCONTINUED**      | 4007 | _Server will respond you with specific message._                                                                      |
| **INVALID_PERMISSIONS**       | 4008 | `permissions` provided in the token are invalid, please don't use `allow_join` or `allow_mod` with `ask_join`.        |
| **START_RECORDING_FAILED**    | 4011 | Recording start request failed due to an unknown error.                                                               |
| **STOP_RECORDING_FAILED**     | 4012 | Recording stop request failed due to an unknown error.                                                                |
| **START_LIVESTREAM_FAILED**   | 4013 | Livestream start request failed due to an unknown error.                                                              |
| **STOP_LIVESTREAM_FAILED**    | 4014 | Livestream stop request failed due to an unknown error.                                                               |
| **INVALID_LIVESTREAM_CONFIG** | 4015 | Livestream 'outputs' configuration provided was invalid.                                                              |
| **RECORDING_FAILED**          | 5001 | Recording stopped due to an unknown error.                                                                            |
| **LIVESTREAM_FAILED**         | 5002 | Livestream stopped due to an unknown error.                                                                           |

