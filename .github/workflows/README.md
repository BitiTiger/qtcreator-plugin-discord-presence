# GitHub Actions & Workflows

The `build_cmake.yml` in this directory adds a [GitHub action][1] and workflow that builds
your plugin anytime you push commits to GitHub on Windows, Linux and macOS.

The build artifacts can be downloaded from GitHub and be installed into an existing Qt Creator
installation.

When you push a tag, the workflow also creates a new release on GitHub.

## Keeping it up to date

Near the top of the file you find a section starting with `env:`.

The value for `QT_VERSION` specifies the Qt version to use for building the plugin.

The value for `QT_CREATOR_VERSION` specifies the Qt Creator version to use for building the plugin.

The value for `QT_CREATOR_SNAPSHOT` can either be `NO` or `latest` or the build ID of a specific
snapshot build for the Qt Creator version that you specified.

You need to keep these values updated for different versions of your plugin, and take care
that the Qt version and Qt Creator version you specify are compatible.

Sometimes the CI will fail during the `Configure & Build` step and the error
will say:
```txt
CMake Error at cmake-3.28.3-linux-x86_64/share/cmake-3.28/Modules/CMakeFindDependencyMacro.cmake:76 (find_package):
  By not providing "FindQt6.cmake" in CMAKE_MODULE_PATH this project has
  asked CMake to find a package configuration file provided by "Qt6", but
  CMake did not find one.
```
This most likely happened due to a 404 error in the `Download Qt` step. The
variable `qt_package_suffix` will need to be updated. Visit the link stored
inside the `qt_base_url` variable and update `qt_package_suffix` accordingly.

## What it does

The build job consists of several steps:

* Install required packages on the build host
* Download, unpack and install the binary for the Qt version
* Download and unpack the binary for the Qt Creator version
* Build the plugin and upload the plugin libraries to GitHub
* If a tag is pushed, create a release on GitHub for the tag, including zipped plugin libraries
  for download

## Limitations

If your plugin requires additional resources besides the plugin library, you need to adapt the
script accordingly.

[1]: https://help.github.com/en/actions/automating-your-workflow-with-github-actions/about-github-actions
