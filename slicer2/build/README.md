## Slicer 2 Dependencies
We make use of VCPKG to supply prebuilt dependencies for Windows developers. The general usage is:
- Download the appropriate archive for your project.
- Extract to the `contrib` folder.
- Rename the extracted folder to `vcpkg`.

For more information on usage, check your project's `README.md` or wiki.

For general details on our usage of VCPKG to create these bundles, check [here](https://github.com/mdfbaam/slicer2view/wiki/VCPKG).

## Downloading
Some of the above bundles may fail to download because of a `Error 503 Response object too large` error. In this case,
you will need to download the object using `LFS` directly.

For command line `git`, the process is as follows to download a single `LFS` object:
```
# On Windows (powershell / cmd.exe)
> set GIT_LFS_SKIP_SMUDGE=1
# On Linux (bash)
> export GIT_LFS_SKIP_SMUDGE=1

> git clone git@github.com:mdfbaam/prebuilt.git
> cd prebuilt
> git lfs pull --include [your_desired bundle]
```

Other clients (such as Github Desktop) will also work, but you may be required to download the entire repository.
