# Python client Library for encoding Videos with Coconut

## Install

```console
sudo easy_install coconutpy
```

## Submitting the job

Use the [API Request Builder](https://app.coconut.co/job/new) to generate a config file that match your specific workflow.

Example of `coconut.conf`:

```ini
var s3 = s3://accesskey:secretkey@mybucket

set webhook = http://mysite.com/webhook/coconut?videoId=$vid

-> mp4  = $s3/videos/$vid.mp4
-> webm = $s3/videos/$vid.webm
-> jpg_300x = $s3/previews/thumbs_#num#.jpg, number=3
```

Here is the ruby code to submit the config file:

```python
import coconut
from coconut import job

job = coconut.job.create(
  api_key='k-api-key',
  conf='coconut.conf',
  source='http://yoursite.com/media/video.mp4',
  vars={'vid': 1234}
)

if job['status'] == 'ok':
  print job['id']
else:
  print job['error_code']
  print job['error_message']
```

You can also create a job without a config file. To do that you will need to give every settings in the method parameters. Here is the exact same job but without a config file:

```python
vid = 1234
s3 = 's3://accesskey:secretkey@mybucket'

job = coconut.job.create(
  api_key='k-api-key',
  source='http://yoursite.com/media/video.mp4',
  webhook='http://mysite.com/webhook/coconut?videoId=' + str(vid),
  outputs={
    'mp4': s3 + '/videos/video_' + str(vid) + '.mp4',
    'webm': s3 + '/videos/video_' + str(vid) + '.webm',
    'jpg_300x': s3 + '/previews/thumbs_#num#.jpg, number=3'
  }
)
```

Note that you can use the environment variable `COCONUT_API_KEY` to set your API key.

*Released under the [MIT license](http://www.opensource.org/licenses/mit-license.php).*

---

* Coconut website: http://coconut.co
* API documentation: http://coconut.co/docs
* Github: http://github.com/opencoconut/coconut.rb
* Contact: [support@coconut.co](mailto:support@coconut.co)
* Twitter: [@OpenCoconut](http://twitter.com/opencoconut)