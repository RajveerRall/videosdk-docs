---
sidebar_position: 1
---

# List All Files

## Using list all files API

This API is useful for listing all the files uploaded on cloud.

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
  --url 'https://api.videosdk.live/v1/files/?page=1&perPage=20' \
  --header 'Authorization: `jwt token goes here`'
```

</TabItem>
<TabItem value="node">

```js
const fetch = require("node-fetch");

const url = "https://api.videosdk.live/v1/files/?page=1&perPage=20";
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

url = "https://api.videosdk.live/v1/files"

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

url = URI("https://api.videosdk.live/v1/files/?page=1&perPage=20")

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
    "lastPage": 1
  },
  "data": [
    {
        "meta": {
            "resolution": {
                "width": 720,
                "height": 1280
            },
            "format": "mov,mp4,m4a,3gp,3g2,mj2",
            "duration": 20.032
        },
        "jobId": null,
        "filePath": "files/videos/6052e0064b442a2f16018373.mp4",
        "size": 3965342,
        "type": "video",
        "createdAt": "2021-03-18T05:07:18.771Z",
        "updatedAt": "2021-03-18T05:07:18.771Z",
        "fileUrl": "https://cdn.zujonow.com/files/videos/6052e0064b442a2f16018373.mp4",
        "id": "6052e0064b442a2f16018374"
    },
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
      <MethodListHeading heading="Properties" />
      <MethodListItemLabel name="page" option={"optional"} type={"number"} defaultValue="1" />
      <MethodListItemLabel name="perPage" option={"optional"} type={"number"} defaultValue="20" />
    </MethodListGroup>
  </MethodListItemLabel>
</MethodListGroup>

### Response

<MethodListGroup>
  <MethodListItemLabel name="__response"  type={"object"} >
    <MethodListGroup>
      <MethodListHeading heading="Properties" />
      <MethodListItemLabel name="pageInfo" type={"object"} >
        <MethodListGroup>
          <MethodListItemLabel name="currentPage"  type={"number"} />
          <MethodListItemLabel name="perPage"  type={"number"} />
          <MethodListItemLabel name="lastPage" type={"number"} />
        </MethodListGroup>
      </MethodListItemLabel>
      <MethodListItemLabel name="data" type={"Array<object>"} >
        <MethodListItemLabel name="meta" type={"object"} >
          <MethodListGroup>
          <MethodListGroup>
            <MethodListItemLabel name="resolution"  type={"object"} >
              <MethodListItemLabel name="width"  type={"number"} />
              <MethodListItemLabel name="height"  type={"number"} />
            </MethodListItemLabel>
          </MethodListGroup>
          <MethodListItemLabel name="format"  type={"string"} />
          <MethodListItemLabel name="duration"  type={"number"} />
          </MethodListGroup>
        </MethodListItemLabel>
        <MethodListItemLabel name="jobId"  type={"string"} />
        <MethodListItemLabel name="filePath"  type={"string"} />
        <MethodListItemLabel name="size"  type={"number"} />
        <MethodListItemLabel name="type"  type={"string"} />
        <MethodListItemLabel name="createdAt"  type={"date"} />
        <MethodListItemLabel name="updatedAt"  type={"date"} />
        <MethodListItemLabel name="fileUrl"  type={"string"} />
        <MethodListItemLabel name="id"  type={"string"} />
      </MethodListItemLabel>
    </MethodListGroup>
  </MethodListItemLabel>
</MethodListGroup>
