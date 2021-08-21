---
title: 'Laravel and Digital Ocean Spaces Storage'
excerpt: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Praesent elementum facilisis leo vel fringilla est ullamcorper eget. At imperdiet dui accumsan sit amet nulla facilities morbi tempus.'
coverImage: '/assets/blog/preview/cover.jpg'
date: '2020-03-16T05:35:07.322Z'
author:
  name: Joe Haddad
  picture: 'https://s3.us-west-2.amazonaws.com/secure.notion-static.com/4099f5bd-ac53-4a90-bdc6-2e6fda7d907d/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210821%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210821T063818Z&X-Amz-Expires=86400&X-Amz-Signature=55585a9575ada0539b515354fa20bbb992c3649bad0f7d5ca7dbbe41838f8906&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22'
ogImage:
  url: 'https://s3.us-west-2.amazonaws.com/secure.notion-static.com/4099f5bd-ac53-4a90-bdc6-2e6fda7d907d/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210821%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210821T063818Z&X-Amz-Expires=86400&X-Amz-Signature=55585a9575ada0539b515354fa20bbb992c3649bad0f7d5ca7dbbe41838f8906&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22'
---

Using Composer install the S3 Storage Driver

```bash
composer require league/flysystem-aws-s3-v3
```

For performance it is recommended to use a cached adapter.

```bash
composer require league/flysystem-cached-adapter
```

Open the `config/filesystems.php` configuration file. After the S3 driver array configuration add the following code:

```bash
'spaces' => [
    'driver' => 's3',
    'key' => env('DO_SPACES_KEY'),
    'secret' => env('DO_SPACES_SECRET'),
    'endpoint' => env('DO_SPACES_ENDPOINT'),
    'region' => env('DO_SPACES_REGION'),
    'bucket' => env('DO_SPACES_BUCKET'),
],
```

Then add the following details to your `.env` file with the corresponding Digital Ocean API details

```bash
DO_SPACES_KEY=[API_KEY]
DO_SPACES_SECRET=[API_SECRET]
DO_SPACES_ENDPOINT=[API_ENDPOINT]
DO_SPACES_REGION=[API_REGION]
DO_SPACES_BUCKET=[API_BUCKET]
```
Now you can use the Laravel Storage system as normal, be sure to specify the location of the storage, as `disk('spaces')` as seen in the example below, here's an example test you can add to your routes `web.php` file.

```bash
Route::get('/file-upload', function () {
    return Storage::disk('spaces')->get('file.txt', 'Contents belong here');
});

Route::get('/files-read', function () {
    return Storage::disk('spaces')->get('file.txt');
});
```