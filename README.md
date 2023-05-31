# triangle-injection
This is a very small proof of concept project, demonstrating how to write a proxy dll file that will intercept D3D11 calls, and inject new graphics commands in a game that you otherwise don't have source access to.

Details about how this project works can be found on [this blog post](http://kylehalladay.com/blog/2021/07/14/Dll-Search-Order-Hijacking-For-PostProcess-Injection.html)

The solution file builds a test app, which renders a spinning cube, and a proxy d3d11.dll file. When that file (and associated shader files) are placed in the same directory as a D3D11 application's binary file, that DLL will intercept calls to D3D11CreateDeviceAndSwapChain, and issue new graphics commands that result in a triangle being drawn across half the screen.

This is intended as a minimal example only and has been made intentionally less functional in order to make it easier to understand.

The DLL requires that the target app uses D3D11CreateDeviceAndSwapChain to create the associated D3D11 objects. If an app instead uses different API calls (like CreateDevice()), attempting to use this proxy dll will cause the program to crash. It also doesn't play nicely with the D3D11 debug layers. For an example of a more fully featured / production quality graphics injector, check out [Reshade](https://reshade.me)


![Screenshot of Skyrim with triangle drawn over top](https://github.com/khalladay/triangle-injection/blob/main/skyrim.jpg?raw=true)

![Screenshot of test app with triangle drawn over top](https://github.com/khalladay/triangle-injection/blob/main/test_app.png?raw=true)

## Build

* Prepare Visual Studio
  * VS 2019 or later
  * Install `MSVC v142 - VS 2019 C++ x64/86 build tools`
* Open the solution
  * Re-target the solution to use `MSVC v142` if needed
* Build the solution
  * Both `d3d11_proxy_dll` and `d3d11_test_app` are built
  * Output will be located on `build/x64Debug/`
    * `content/`
    * `hook_content/`
    * `d3d11.dll`: proxy DLL
    * `d3d11.exp`
    * `d3d11.lib`
    * `d3d11.pdb`
    * `d3d11_test_app.exe`: test app showing rotating cube
    * `d3d11_test_app.pdb`

## Hook

* For test app
  * Execute `d3d11_test_app.exe`
* For your app
  * Copy DLL and related files below to the directory where your app's executable locates
    * `content/`, `hook_content/`, `d3d11.dll`, `d3d11.exp`, `d3d11.lib`
  * Execute your app
