## Adding New Dependencies in VCPKG

If you would like to add a new dependency or tweak existing dependencies, use the following steps.

1. Download and install your compiler of choice.
    - Some compilers behave better with VCPKG than others. For instance, MingW64 has issues compiling OpenVDB.
    - For recipes that are "guaranteed" to build, use MSVC 2022.

2. Download VCPKG from our internal fork: https://github.com/mdfbaam/vcpkg
    - By forking, we have pinned our dependencies to their current versions. If you would like to use a newer
      package definitions, see [Updating Dependencies in VCPKG](#updating-dependencies-in-vcpkg)
    - Due to the path length limitation in Windows, you should download your VCPKG install to a near top-level
      path - something like `C:/vcpkg/` or `C:/devel/vcpkg/`. Not doing so may cause some builds to fail.
      
3. Bootstrap VCPKG.
    - Open a Powershell and navigate to the directory where you installed VCPKG.
    - Run the following to bootstrap VCPKG:
      ```
      > bootstrap-vcpkg.bat
      ```
    - This will install `vcpkg.exe` into the current directory.
    
4. Install dependencies with VCPKG.
    - In the same Powershell instance as before, run the following command to install all required dependencies:
      ```
      > vcpkg --triplet x64-windows install [your_new_dependency] assimp libarchive boost cgal openvdb nlohmann-json eigen3 qt5 qt5-charts
      ```
    - This will take a very long time to build.
    - Add any extra dependencies you wish to this invocation. You can find available packages by running:
      ```
      > vcpkg search [your_query]
      ```
    - If you are running this behind a firewall (e.g. ORNL's firewall), some packages may fail to download completely.
      In this case, you should download the failed sources manually to the $VCPKG_ROOT/downloads folder. The packages 
      with known download failures are:
      ```
      https://mirrors.ocf.berkeley.edu/qt/official_releases/jom/jom_1_1_3.zip - copy and rename the copy to jom_1_1_3-5b158ead.zip, both copies are needed.
      https://www.python.org/ftp/python/3.10.7/python-3.10.7-embed-amd64.zip
      ```
    
5. Export a new zip file with the built files:
    - Run:
      ```
      > vcpkg --triplet x64-windows export --zip [your_new_dependency] assimp libarchive boost cgal openvdb nlohmann-json eigen3 qt5 qt5-charts
      ```
    - Delete the extracted dependencies folder in slicer2view at `contrib/vcpkg` and replace it with the new contents of the exported zip.
    - Re-run the CMake for slicer2view.
    
If you just wish to test the dependency instead of packaging it, you can also temporarily set `CMAKE_TOOLCHAIN_FILE` in `scripts/vcpkg/vcpkg.cmake` to 
the vcpkg.cmake from your local installation of VCPKG. Remember to change this back when you properly export your dependencies.

## Updating Dependencies in VCPKG

Since we have an internal fork of VCPKG, you must update the fork to update the dependencies. 

1. Download VCPKG from our internal fork: https://github.com/mdfbaam/vcpkg
    - Due to the path length limitation in Windows, you should download your VCPKG install to a near top-level
      path - something like `C:/vcpkg/` or `C:/devel/vcpkg/`. Not doing so may cause some builds to fail.
      
2. Sync the fork with upstream.
    - There are a number of ways to do this for different clients.
    - GitHub provides good instructions for Git CLI [here](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/syncing-a-fork#syncing-a-fork-branch-from-the-command-line).
    - You should NOT sync with the WebUI. Test the merge locally first then push the sync to our internal fork.
    
3. Bootstrap VCPKG.
    - Open a Powershell and navigate to the directory where you installed VCPKG.
    - Run the following to bootstrap VCPKG:
      ```
      > bootstrap-vcpkg.bat
      ```
    - This will install `vcpkg.exe` into the current directory.
    
4. Install dependencies with VCPKG.
    - In the same Powershell instance as before, run the following command to install all required dependencies:
      ```
      > vcpkg --triplet x64-windows install assimp libarchive boost cgal openvdb nlohmann-json eigen3 qt5 qt5-charts
      ```
    - This will take a very long time to build.
    - If you are running this behind a firewall (e.g. ORNL's firewall), some packages may fail to download completely.
      In this case, you should download the failed sources manually to the $VCPKG_ROOT/downloads folder. The packages 
      with known download failures are:
      ```
      https://mirrors.ocf.berkeley.edu/qt/official_releases/jom/jom_1_1_3.zip - copy and rename the copy to jom_1_1_3-5b158ead.zip, both copies are needed.
      https://www.python.org/ftp/python/3.10.7/python-3.10.7-embed-amd64.zip
      ```
      
5. Export a new zip file with the built files:
    - Run:
      ```
      > vcpkg --triplet x64-windows export --zip assimp libarchive boost cgal openvdb nlohmann-json eigen3 qt5 qt5-charts
      ```
    - Delete the extracted dependencies folder in slicer2view at `contrib/vcpkg` and replace it with the new contents of the exported zip.
    - Re-run the CMake for slicer2view.
