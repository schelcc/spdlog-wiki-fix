This short guide can help you get started with integrating the C++ **spdlog** library into the Eclipse IDE.
The whole configuration consists of 5 simple steps.

Note: We can use git clone. Of course :)<br>
Note: In many cases, it is advisable to choose a parent folder named '_ThirdParty_' or '_libs_' or any other name that immediately tells us that it is a folder with third-party files. I chose as the parent folder, a folder called '_ThirdParty_'...

1) We will copy/unpack the content of the archive, which we previously downloaded from [the project pages](https://github.com/gabime/spdlog), into the newly created or already existing project. Or you can clone it from [github](https://github.com/gabime/spdlog)

2) From the menu bar choose `Project > Properties > C/C+ Build > GCC C++ Compiler > Preprocessor`, and here you have to add a definition by clicking the Add button (define symbol `-D`) and add the following definition `SPDLOG_COMPILED_LIB`. Attention, keep the size of the letters!

3) Then click `Includes` (We are still here:` Project > Properties > C/C++ Build > GCC C++ Compiler`) and add the path to the folder with headers for the library **spdlog**.
For example: `thirdParty/spdlog/include`

4) Click on `apply and close`.

5) Click `Project > Indexer > rebuild`


Happy Coding ;) 