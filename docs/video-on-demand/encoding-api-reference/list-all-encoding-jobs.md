---
sidebar_position: 1
---

# List Encoding Job

### Using list encoding job API

List down all the encoding jobs.

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

<Tabs
defaultValue="curl"
values={[
{label: 'cURL', value: 'curl'},
{label: 'NodeJS/JS', value: 'node'},
{label: 'Python', value: 'python'},
{label: 'Ruby', value: 'ruby'},
{label: 'RESULT', value: 'result'},
]}>
<TabItem value="curl">

```js
curl --request GET \
  --url 'https://api.videosdk.live/v1/encoder/jobs/?page=1&perPage=20' \
  --header 'Authorization: `jwt token goes here`'
```

</TabItem>
<TabItem value="node">

```js
const fetch = require("node-fetch");

const url = "https://api.videosdk.live/v1/encoder/jobs/?page=1&perPage=20";
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

url = "https://api.videosdk.live/v1/encoder/jobs"

querystring = {"page":"1","perPage":"25"}

headers = {"Accept": "application/json", "Authorization": "jwt token goes here"}

response = requests.request("GET", url, headers=headers, params=querystring)

print(response.text)
```

</TabItem>
<TabItem value="ruby">

```ruby
require 'uri'
require 'net/http'
require 'openssl'

url = URI("https://api.videosdk.live/v1/encoder/jobs/?page=1&perPage=20")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true

request = Net::HTTP::Get.new(url)
request["Accept"] = 'application/json'
request["Authorization"] = 'jwt token goes here'

response = http.request(request)
puts response.read_body
```

</TabItem>
<TabItem value="result">

```js
{
  "pageInfo": {
    "currentPage": 1,
    "perPage": 20,
    "lastPage": 11
  },
  "data": [
    {
      "status": "completed",
      "videoId": "604efc059036d077e3bd03bc",
      "presets": [
        {
          "resolutions": ["240", "360", "480"],
          "id": "604efc0bf2bfa894b27133a0",
          "format": "hls"
        },
        {
          "resolutions": ["360"],
          "id": "604efc0bf2bfa894b27133a1",
          "format": "mp4"
        }
      ],
      "webhookUrl": "https://<your-website-address>/<path>",
      "thumbnails": [
        {
          "resolutions": ["360"],
          "formats": ["jpg", "webp"],
          "filters": ["none", "blur"],
          "id": "604efc0bf2bfa894b27133a2",
          "timestamp": "00:00:03"
        }
      ],
      "createdAt": "2021-03-15T06:17:47.622Z",
      "updatedAt": "2021-03-15T06:18:06.873Z",
      "startedAt": "2021-03-15T06:17:50.626Z",
      "finishedAt": "2021-03-15T06:18:06.871Z",
      "files": [
        {
          "meta": {
            "resolution": {
              "width": 360,
              "height": 640
            },
            "format": "mov,mp4,m4a,3gp,3g2,mj2",
            "duration": 20.04
          },
          "jobId": "604efc0bf2bfa894b271339f",
          "filePath": "files/videos/604efc189036d077e3bd03bd.mp4",
          "size": 1572953,
          "type": "video",
          "createdAt": "2021-03-15T06:18:00.306Z",
          "updatedAt": "2021-03-15T06:18:00.306Z",
          "fileUrl": "https://cdn.zujonow.com/files/videos/604efc189036d077e3bd03bd.mp4",
          "id": "604efc189036d077e3bd03be"
        },
        {
          "meta": {
            "resolution": {
              "width": 480,
              "height": 854
            },
            "format": "hls",
            "duration": 20.04
          },
          "jobId": "604efc0bf2bfa894b271339f",
          "filePath": "files/videos/604efc199036d077e3bd03c0",
          "size": 5755392,
          "type": "x-tar",
          "createdAt": "2021-03-15T06:18:01.455Z",
          "updatedAt": "2021-03-15T06:18:01.455Z",
          "fileUrl": "https://cdn.zujonow.com/files/videos/604efc199036d077e3bd03c0/index.m3u8",
          "id": "604efc199036d077e3bd03c1"
        },
        {
          "jobId": "604efc0bf2bfa894b271339f",
          "filePath": "files/images/604efc1a9036d077e3bd03c2.jpg",
          "size": 24155,
          "type": "image",
          "createdAt": "2021-03-15T06:18:02.966Z",
          "updatedAt": "2021-03-15T06:18:02.966Z",
          "fileUrl": "https://cdn.zujonow.com/files/images/604efc1a9036d077e3bd03c2.jpg",
          "id": "604efc1a9036d077e3bd03c3"
        },
        {
          "jobId": "604efc0bf2bfa894b271339f",
          "filePath": "files/images/604efc1b9036d077e3bd03c4.jpg",
          "size": 3647,
          "type": "image",
          "createdAt": "2021-03-15T06:18:03.810Z",
          "updatedAt": "2021-03-15T06:18:03.810Z",
          "fileUrl": "https://cdn.zujonow.com/files/images/604efc1b9036d077e3bd03c4.jpg",
          "id": "604efc1b9036d077e3bd03c5"
        },
        {
          "jobId": "604efc0bf2bfa894b271339f",
          "filePath": "files/images/604efc1d9036d077e3bd03c6.webp",
          "size": 14976,
          "type": "image",
          "createdAt": "2021-03-15T06:18:05.896Z",
          "updatedAt": "2021-03-15T06:18:05.896Z",
          "fileUrl": "https://cdn.zujonow.com/files/images/604efc1d9036d077e3bd03c6.webp",
          "id": "604efc1d9036d077e3bd03c7"
        },
        {
          "jobId": "604efc0bf2bfa894b271339f",
          "filePath": "files/images/604efc1e9036d077e3bd03c8.webp",
          "size": 1296,
          "type": "image",
          "createdAt": "2021-03-15T06:18:06.711Z",
          "updatedAt": "2021-03-15T06:18:06.711Z",
          "fileUrl": "https://cdn.zujonow.com/files/images/604efc1e9036d077e3bd03c8.webp",
          "id": "604efc1e9036d077e3bd03c9"
        }
      ],
      "id": "604efc0bf2bfa894b271339f"
    }
    ...
  ]
}
```

</TabItem>
</Tabs>

import MethodListGroup from '@theme/MethodListGroup';
import MethodListItemLabel from '@theme/MethodListItemLabel';
import MethodListHeading from '@theme/MethodListHeading';

### Request

<MethodListGroup>
  <MethodListItemLabel name="__request" option={"required"} type={"object"} >
    <MethodListGroup>
      <MethodListHeading heading="Request" />
      <MethodListItemLabel name="page" option={"optional"} type={"number"} defaultValue="1" />
      <MethodListItemLabel name="perPage" option={"optional"} type={"number"} defaultValue="20" />
    </MethodListGroup>
  </MethodListItemLabel>
</MethodListGroup>

### Response

<MethodListGroup>
  <MethodListItemLabel name="__response"  type={"object"} >
    <MethodListGroup>
      <MethodListHeading heading="Response" />
      <MethodListItemLabel name="pageInfo" type={"object"} >
        <MethodListGroup>
          <MethodListItemLabel name="currentPage"  type={"number"} />
          <MethodListItemLabel name="perPage"  type={"number"} />
          <MethodListItemLabel name="lastPage" type={"number"} />
        </MethodListGroup>
      </MethodListItemLabel>
      <MethodListItemLabel name="data" type={"Array<object>"} >
        <MethodListItemLabel name="id" type={"string"} />
        <MethodListItemLabel name="status"  type={"string"} />
        <MethodListItemLabel name="videoId"  type={"string"} />
        <MethodListItemLabel name="presets" type={"Array<object>"} >
          <MethodListGroup>
            <MethodListItemLabel name="resolutions" description={"Possible values are 240, 360, 720, 1080 and 4k"}  type={"Array<string>"}  >
            </MethodListItemLabel>
            <MethodListItemLabel name="format"  type={"string"} />
            <MethodListItemLabel name="id"  type={"string"} />
          </MethodListGroup>
        </MethodListItemLabel>
        <MethodListItemLabel name="thumbnails" option={"optional"} type={"Array<object>"} >
          <MethodListGroup>
            <MethodListItemLabel name="timestamp" type={"string"} />
            <MethodListItemLabel name="resolutions" type={"Array<string>"}  >
            </MethodListItemLabel>
            <MethodListItemLabel name="formats" type={"Array<string>"}  >
            </MethodListItemLabel>
            <MethodListItemLabel name="filters" type={"Array<string>"}  >
            </MethodListItemLabel>
          </MethodListGroup>
        </MethodListItemLabel>
        <MethodListItemLabel name="webhookUrl" type={"string"} />
        <MethodListItemLabel name="files" type={"Array<object>"} >
          <MethodListGroup>
            <MethodListItemLabel name="meta" type={"object"}>
              <MethodListItemLabel name="resolution" type={"object"} >
                <MethodListItemLabel name="width" type={"number"} />
                <MethodListItemLabel name="height" type={"number"} />
              </MethodListItemLabel>
            </MethodListItemLabel>
            <MethodListItemLabel name="format" type={"string"} />
            <MethodListItemLabel name="duration" type={"number"} />
          </MethodListGroup>
          <MethodListItemLabel name="jobId" type={"string"} />
          <MethodListItemLabel name="filePath" type={"string"} />
          <MethodListItemLabel name="size" type={"number"} />
          <MethodListItemLabel name="type" type={"string"} />
          <MethodListItemLabel name="createdAt" type={"date"} />
          <MethodListItemLabel name="updatedAt" type={"date"} />
          <MethodListItemLabel name="fileUrl" type={"string"} />
          <MethodListItemLabel name="id" type={"string"} />
        </MethodListItemLabel>
      </MethodListItemLabel>
    </MethodListGroup>
  </MethodListItemLabel>
</MethodListGroup>
