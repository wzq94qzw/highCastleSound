# UE4_BoilerplatePlugin
Boilerplate code for a C++/BP plugin that handles dependencies automatically.

## Setup

1. In Terminal, navigate to your Unreal project's "Plugins" folder (create one if it does not exist already) and run this command:
> ❗️ If you don't have Unreal Project, set it up first (basic C++ project will suffice). This project will be your *plugin development* (PluginDev) project and should be published under your github account.
```
bash <(curl -s https://raw.githubusercontent.com/peetonn/UE4_BoilerplatePlugin/master/bootstrap.sh)
```

2. When asked, provide your plugin name (do **not** use words "*boilerplate*", *"Plugin"* or *"Module"* in your plugin name) and hit Enter.
> 1. The plugin code and structure will be generated and new git commit will be created. You must add new remote for your Plugin repo. Follow the instructions on adding git remote shown on the screen.
> 2. You should also add your plugin repo as a git submodule to your PluginDev repo.

3. Clone [DDTools](https://github.com/remap/DDTools) into your Plugins folder **recursively**:
```
git clone --recursive https://github.com/remap/DDTools
```

4. Open your PluginDev project, navigate to your plugin's *Content/UI* folder, and rename **"BP_ModulePanel"** to **"BP_\<your plugin name>ModulePanel"**
5. Open Plugins window in the Editor (Settings -> Plugins) and enable your plugin.
6. Restart your project. It will prompt you to recompile found plugins, click "Yes".

This will create main module code for your plugin. You can now start adding code to your plugin.

## Quick Start with the Boilerplate Code

After generation is complete, your plugin will be organized as follows:
```
YourPlugin/
├── Config
├── Content   // you can add arbitrary content required by your plugin here
│   └── UI
│       └── BP_YourPluginModulePanel.uasset   // your plugin UI panel, use it if your plugin needs debug UI
├── README.md   // add information about your plugin and how to use it here
├── Resources
├── Source
│   ├── YourPlugin        // main module of your plugin; you can add more modules into your plugin if necessary
│   │   ├── Private       // all private internals go here 
│   │   │   ├── YourPlugin.cpp
│   │   │   └── git-describe.h
│   │   ├── Public        // all public plugin interfaces go here
│   │   │   └── YourPlugin.h 
│   │   └── YourPlugin.Build.cs
│   ├── YourPluginTest    // test code for your module should go here
│   │   ├── Private
│   │   │   ├── YourPluginTest.cpp
│   │   │   └── git-describe.h
│   │   ├── Publi
│   │   │   └── YourPluginTest.h 
│   │   └── YourPluginTest.Build.cs
│   └── ThirdParty
│       └── depsTest1   // module for adding arbitrary third-party dependencies
│           ├── depsTest1.Build.cs
│           └── depsTest1.tps
└── YourPlugin.uplugin
```

See the notes on the diagram above.

1. You can start adding code, content or blueprints to your plugin:
* **Code** goes into *Source/YourPlugin* module (or add more modules if you need to modularize for better structure).
* **Blueprints** go into *Content/Blueprints* folder.
* **Assets** go into *Content/Assets* folder.
* **Test Blueprints** go into *Content/Test/Blueprints*
* **Test Assets** go into *Content/Test/Assets*
2. Add sample code or any examples on how to use your plugin.
> 😃 Use **YourPluginTest** to do you main development and testing of the plugin. This can also serve as an example code on how to use it.

### Debug UI

By default, your plugin will be set up with Debug UI widget (*Content/UI/BP_YourPluginMpdulePanel.uasset*) which is loaded automatically by the shell app code and shows information about your plugin, such as name, version, build type, etc.
You can use this panel to add more debug/status UIs related to your plugin.

## Adding Third Party Dependencies

If your plugin requires any C++ third party libraries, the boilerplate code is set up to handle them automatically, **as long as third party code and binaries organized in specific way**, described below.
Your third party dependency may either be *header-only* or *dynamic library* (which contains also binaries, pre-built for a certain platform, or several platforms).
In either case, you need to organize your third party dependency in a folder like this:

```
<third-party lib name>
  *-- include      // headers should be placed here
  *-- lib          // (can omit this subfolder for header-only dependencies)
    *-- Android    // include only those platforms, that your third-party supports
    *-- IOS
    *-- macOS
    *-- Win64
```

You should place your third party folder under `<plugin name>/ThirdParty/deps<plugin name>` folder. The [build script](https://github.com/peetonn/UE4_BoilerplatePlugin/blob/master/Source/ThirdParty/depsBoilerplate/depsBoilerplate.Build.cs) will pick it up from there for proper compling.

## Manual setup
If, for some reason, bootstrapping instructions above didn't work, here are the instructions on manual setup.

1. Clone the repo:
```
git clone https://github.com/peetonn/UE4_BoilerplatePlugin.git
```

2. Rename cloned directory into your **plugin name** (substitute `<plugin name>` with your **plugin name**, for example *OSCSupport*).

> **Do not use word "boilerplate" in your plugin name** 

```
mv UE4_BoilerplatePlugin <plugin name>
```

3. Execute these commands (substitute `<plugin name>` with your **plugin name**):

```
cd <plugin name>
./setup.sh `basename $(pwd)`
```

4. Once the setup is completed, copy your plugin folder over to your UE4 project's "Plugins" folder (create this folder, if it does not exist already).
5. Restart your project, it will prompt you to recompile found plugins, click "Yes".

This will create main module code for your plugin. You can now start adding code to your plugin.
