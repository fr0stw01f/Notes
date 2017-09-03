# Building chromium for Android

## Install the depot_tools package
1. Confirm git and python are installed. git 2.2.1+ recommended. python 2.7+ recommended.

2. Fetch depot_tools:

```sh
$ git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git
```

3. Add depot_tools to your PATH:

```sh
$ export PATH=`pwd`/depot_tools:"$PATH"
```
  * Yes, you want to put depot_tools ahead of everything else, otherwise gcl will refer to the GNU Common Lisp compiler.
  * You may want to add this to your .bashrc file or your shell's equivalent so that you don’t need to reset your $PATH manually each time you open a new shell.

## Check out the source code

```sh
$ mkdir ~/chromium && cd ~/chromium
~/chromium$ fetch --nohooks android
```

If error occurs, just try again.

```sh
~/chromium/src$ gclient sync
```

Sync to a specific version, identified by a commit hash, chose from [stable versions](https://chromium.googlesource.com/chromium/src/+refs) or at least [lkgr versions](http://chromium-status.appspot.com/revisions)
Here is the [development calendar](http://dev.chromium.org/developers/calendar).

For example, the sha1 of the stable version 56.0.2924.87 is b169b9a1cc402573843e8c952af14c4e43487e91.

~~Stable versions hardly work, don’t know why, try lkgrs.~~

75ee3cee8e33bd1bcf4fb335b20acc04991448e8 is successfully synced.

```sh
~/chromium$ gclient sync --nohooks -r <lkgr-sha1>
~/chromium/src$ gn args out/Default # Default is just a name
```

## Config
Add the following to the file editor: (with modifications depending on purposes).

```sh
target_os = "android"
target_cpu = "x64"  # (default)
is_debug = false  # (default), better choose false to make webview work

# Other args you may want to set:
is_component_build = true
is_clang = true
symbol_level = 1  # Faster build with fewer symbols. -g1 rather than -g2
enable_incremental_javac = true  # Much faster; experimental, need to remove if not debug
```

```sh
~/chromium/src$ build/install-build-deps-android.sh 
~/chromium/src$ . build/android/envsetup.sh
```
## Build

* Build Chrome (chrome/BUILD.gn)

```sh
~/chromium/src$ ninja -C out/Default chrome_public_apk
```

* Build Content Shell (content/BUILD.gn)

```sh
~/chromium/src$ ninja -C out/Default content_shell_apk
```

* Build WebView Shell (android_webview/test/BUILD.gn)

```sh
~/chromium/src$ ninja -C out/Default android_webview_apk
```

* Build System WebView (android_webview/BUILD.gn)

```sh
# Modify android_webview/BUILD.gn to change the package name
~/chromium/src$ ninja -C out/Default system_webview_apk 
```

* Build System WebView Shell (android_webview/tools/system_webview_shell/BUILD.gn)

```sh
~/chromium/src$ ninja -C out/Default system_webview_shell_apk
```
