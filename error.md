## Errors faced during deployment of 5g-core

#### If you get some `crashloopback` error in pods, check out the logs and if logs says `too many open files`, like this
```bash
$ kubectl -n omec logs amf-6dd746b9cd-2mk2j
...
...
} (resolver returned new addresses)
2023/01/24 17:24:56 INFO: [core] [Channel #1] Channel switches to new LB policy "pick_first"
2023/01/24 17:24:56 INFO: [core] [Channel #1 SubChannel #2] Subchannel created
2023/01/24 17:24:56 too many open files
As the message shows, the problem is due to “too many open files”.
```
follow this repo to fix this:

https://github.com/AdityaKoranga/too-many-open-files-error

> **Note**: Mostly you see this error in `amf` and `simapp`