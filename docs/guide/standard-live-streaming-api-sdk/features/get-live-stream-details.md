---
title: Live Streaming Details | Video SDK Docs
hide_title: false
hide_table_of_contents: false
description: Live Streaming Details documentation offers the most flexibility and control over your live viewing experience, build a custom integration with your live streaming web & app.
sidebar_label: Get Live Stream Details
pagination_label: Get Live Stream Details
keywords:
  - live streaming
  - live broadcasting
  - interactive live streaming
  - live audio streaming
image: img/videosdklive-thumbnail.jpg
sidebar_position: 1
slug: get-live-stream-details
---

# Live Stream - Get Live Stream Details

This guide will provide an overview of retrieving particular live stream details using live stream id, which we have created in [Create New Live Stream](/docs/guide/standard-live-streaming-api-sdk/features/create-new-live-stream).

### Query Params

| Property Name | Type   | Description               |
| ------------- | ------ | ------------------------- |
| id            | string | unique id of live stream. |

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

<Tabs
defaultValue="curl"
values={[
{label: 'cURL', value: 'curl'},
{label: 'NodeJS/JS', value: 'node'},
{label: 'Python', value: 'python'},
{label: 'Ruby', value: 'ruby'},
{label: 'RESPONSE', value: 'response'},
]}>
<TabItem value="curl">

```js
curl --request GET \
  --url 'https://api.videosdk.live/v1/livestreams/${id}' \
  --header 'Authorization: `jwt token goes here`'
```

</TabItem>
<TabItem value="node">

```js
const fetch = require("node-fetch");

const url = "https://api.videosdk.live/v1/livestreams/${id}";
const options = {
  method: "GET",
  headers: { Accept: "application/json", Authorization: `jwt token goes here` },
};

fetch(url, options)
  .then((res) => res.json())
  .then((json) => console.log(json))
  .catch((err) => console.error("error:" + err));
```

</TabItem>
<TabItem value="python">

```python
import requests

url = "https://api.videosdk.live/v1/livestreams/${id}"

headers = {"Accept": "application/json", "Authorization": "jwt token goes here"}

response = requests.request("GET", url, headers=headers)

print(response.text)
```

</TabItem>
<TabItem value="ruby">

```ruby
require 'uri'
require 'net/http'
require 'openssl'

url = URI("https://api.videosdk.live/v1/livestreams/${id}")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true

request = Net::HTTP::Get.new(url)
request["Accept"] = 'application/json'
request["Authorization"] = 'jwt token goes here'

response = http.request(request)
puts response.read_body
```

</TabItem>
<TabItem value="response">

```js
{
  "record": true,
  "id": "6034a7dfa9cedd724c20cf67",
  "name": "Nickname for livestream",
  "streamKey": "d492a9ed-84ce-448c-84dd-718bdea724a5",
  "upstreamUrl": "rtmp://live.videosdk.live/live/...",
  "downstreamUrl": "https://live.videosdk.live/live/.../index.m3u8",
  "recordingUrl": "https://live.videosdk.live/live/.../storage/index.m3u8",
}
```

</TabItem>
</Tabs>

import MethodListGroup from '@theme/MethodListGroup';
import MethodListItemLabel from '@theme/MethodListItemLabel';
import MethodListHeading from '@theme/MethodListHeading';

<MethodListGroup>
  <MethodListItemLabel  description="Response Body" >
    <MethodListGroup>
      <MethodListHeading heading="parameters" />
      <MethodListItemLabel name="id"  type={"string"}  description="Unique identifier of live stream." />
      <MethodListItemLabel name="name"  type={"string"}  description="Provided name of the live stream." />
      <MethodListItemLabel name="record"  type={"boolean"}  description="Flag for live stream recording, which you have provided while creating live stream." />
      <MethodListItemLabel name="streamKey"  type={"string"}  description="Stream keys are like your live stream’s password and address" />
      <MethodListItemLabel name="upstreamUrl"  type={"string"}  description="where a RTMP stream is used to send video between an encoder and server." />
      <MethodListItemLabel name="downstreamUrl"  type={"string"}  description="It's URL, Where you can play live stream in video player (Support HLS format)." />
      <MethodListItemLabel name="recordingUrl"  type={"string"}  description="It's URL, Where live stream recording is stored." />
      <MethodListItemLabel name="restream"  type={"array"}  description="This property contains object of RTMP url and streamKey, which you have provided in body params." />
      
    </MethodListGroup>
  </MethodListItemLabel>
</MethodListGroup>
