---
title: Authorize API
layout: single
type: posts
excerpt: "Filewall Authorize API reference"
sidebar:
    nav: "posts"
categories: api-docs filewall
permalink: /:categories/:title
---

# Authorize 

Sends an authorized API key and desired result file settings. 

Returns an upload URL and file information. 

## Request

`https://dev.filewall.io/api/authorize`

### Request Header

|Header Field|Value|
|---|---|
|**apikey**|Required. Valid API key. See [Authorization](/getting-started#authorization) for details.|

### Request Body (Optional)

<table class="table">

<thead>
    <tr>
        <th>Name</th>
        <th>Type</th>
        <th>Description</th>
        <th>Notes</th>
    </tr>
</thead>
<tbody>
    <tr>
        <td><b>max_pages_per_document</b></td>
        <td>integer</td>
        <td>Maximum number of pages to convert for the document.</td>
        <td>Used to limit the number of pages per document that are converted. Useful when large documents are not expected, to reduce workloads and costs.</td>
    </tr>
    <tr>
        <td><b>max_files_per_archive</b></td>
        <td>integer</td>
        <td>Maximum number of archived files to extract.</td>
        <td>If the uploaded file is an archive (`.zip` or `tar.gz`), use this to limit how many files are extracted from the archive for conversion and repacking into a new archive.</td>
    </tr>
    <tr>
        <td><b>max_width</b></td>
        <td>integer</td>
        <td>Maximum width of output images. Units are px.</td>
        <td> </td>
    </tr>
    <tr>
        <td><b>max_height</b></td>
        <td>integer</td>
        <td>Maximum height of output images. Units are px.</td>
        <td> </td>
    </tr>
    <tr>
        <td><b>max_dpi</b></td>
        <td>integer</td>
        <td>Maximum DPI of output images.</td>
        <td> </td>
    </tr>
</tbody>
</table>

## Response

The response is JSON and contains, at minimum, an error code. 

See [Response Codes](#codes) for details. 

### Sample Response
```json
{
    "uid": "abbc7c94-h11y-4r27-92fg-czd3362f464a",
    "status": "new",
    "created": "2020-02-01T14:19:10.713Z",
    "source_name": "",
    "source_size": 0,
    "links": [
        "self": "https://filewall.io/api/get/abbc7c94-h11y-4r27-92fg-czd3362f464a", 
        "upload": "https://filewall.io/api/upload/abcd_very_long_example"
    ]  
}
``` 

### Response Body

<table class="table">

<thead>

<tr>

<th>Name</th>

<th>Type</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>**uid**</td>

<td>string</td>

<td>Unique request ID</td>

</tr>

<tr>

<td>**status**</td>

<td>string</td>

<td>Request status. The default value is `new` during the authorize stage.</td>

</tr>

<tr>

<td>**created**</td>

<td>string</td>

<td>Date and timestamp for when the request was made.</td>

</tr>

<tr>

<td>**source_name**</td>

<td>string</td>

<td>Original file name. Blank at the authorize stage.</td>

</tr>

<tr>

<td>**source_size**</td>

<td>integer</td>

<td>Original file size in bytes. Default is 0 at the authorize stage.</td>

</tr>

<tr>

<td>**links**</td>

<td>array</td>

<td>URLs for various actions. Possible values are: * **self**: Link to call information about the uploaded file. See [Status](/status) for details. * **upload**: Link to which the file must be uploaded. Valid for 60 minutes. See [Upload](/upload) for details.</td>

</tr>

<tr>

<td>**error**</td>

<td>string</td>

<td>Error code. See Response Codes for details. **Note:** This field is only displayed in the response file if an error occurs during the upload process.</td>

</tr>

</tbody>

</table>

### <a name="codes"></a> Response Codes

<table class="table">

<thead>

<tr>

<th>Code</th>

<th>Value</th>

<th>Description</th>

<th>Response Body Value</th>

</tr>

</thead>

<tbody>

<tr>

<td>**202**</td>

<td>Accepted</td>

<td>Request accepted.</td>

<td></td>

</tr>

<tr>

<td>**400**</td>

<td>Bad Request</td>

<td>One of the specified request body parameters was incorrect, or the API key was not sent in the request header.</td>

<td>`invalid_request`</td>

</tr>

<tr>

<td>**402**</td>

<td>Payment Required</td>

<td>Account file limit reached. Upgrade your plan to continue.</td>

<td>`payment_required`</td>

</tr>

<tr>

<td>**403**</td>

<td>Forbidden</td>

<td>Invalid API key in the request header.</td>

<td>`auth_failed`</td>

</tr>

<tr>

<td>406</td>

<td>Not Acceptable</td>

<td>Invalid parameter syntax. For example, _-1_ specified in the DPI parameter in the request.</td>

<td>`value_error`</td>

</tr>

<tr>

<td>**429**</td>

<td>Too Many Requests</td>

<td>Too many simultaneous requests.</td>

<td>`too_many_requests`</td>

</tr>

</tbody>

</table>