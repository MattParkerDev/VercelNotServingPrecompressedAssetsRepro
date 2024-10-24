Deployed repro: https://vercel-not-serving-precompressed-assets-repro.vercel.app/

All the files in the root directory are deployed to vercel. There is no build step for a minimal repro - System.Private.CoreLib.wasm has been pre-compressed to br in the repo.

When you load the website, it will load `System.Private.CoreLib.wasm` automatically.

In the dev tools, you can see that the file is correctly loaded with br encoding, however, it has not used the pre-compressed asset in this repo which is deployed to vercel.

It is easy to verify this behaviour with the difference in file sizes:

| Pre-compressed  | Vercel on the fly compressed |
| ------------- | ------------- |
|  443 KB | 550 KB |

We expect to see a transfer of ~443KB, however we actually get ~550KB:

![image](https://github.com/user-attachments/assets/e166f6d7-4ba2-46a2-9712-d19e9f0af1c8)

Vercel should use existing compressed assets if they exist, as they can be compressed with a higher level of compression at build time once, compared to on the fly where concessions are made for speed vs compression level.
