# How to build MAME with Visual Studio 2022

## Step 1. Set up Visual Studio 2022 with Clang

1. Open the **Visual Studio 2022 Installer**.
2. Ensure the **Desktop Development for C++** Workload checkbox is ticked.
3. Click the **Desktop Development for C++** button. In addition to the preselected defaults, ensure the **Clang tools for Windows** checkbox is also ticked, like so:

![image](https://github.com/user-attachments/assets/deec2e92-2c84-4e55-966a-c201e54bfb32)

4. Install/update Visual Studio.
5. When the install is complete, **don't** open VS 2022 - you don't yet have a VS solution to load. We will create one later.
   

## Step 2. Install MSYS2 Build tools

1. Download the **MSYS2** installer from https://www.msys2.org/
2. Run the installer. For the installation folder, I accepted the default **C:\msys64**, like so:

![image](https://github.com/user-attachments/assets/90bb0c38-7450-4b5b-bb94-089e2cf74714)

If you opt for a different folder, then be sure to use it in the coming steps instead of **c:\msys64**.

3. Complete the install. 
4. Run the MSYS shell (located in **c:\msys64\ucrt64.exe**)
5. When the shell opens, input: **pacman -S mingw-w64-x86_64-gcc mingw-w64-x86_64-make mingw-w64-x86_64-python** , as shown below:

![image](https://github.com/user-attachments/assets/0814810e-d512-4482-9e22-9f7128a30744)

6. Type "y" (no quotes) when asked to **proceed with installation [Y/n]?**
7. pacman will acquire and install the **GCC C++ compiler**, the **Make** build tool and **Python 3** which are all required for the Make process.
8. When the tools have been installed, add **C:\msys64\mingw64\bin** to your PATH environment variable, like so:

![image](https://github.com/user-attachments/assets/5a9d1882-b2fd-4af3-a65c-72edf9ed05eb)


## Step 3. Make the Visual Studio 2022 solution files 

1. Git Clone the MAME Repo if you haven't already.
2. Navigate to where you cloned the repo. In the source root, there should be a file named "makefile" which has no file extension.

![image](https://github.com/user-attachments/assets/994691d0-c4ba-48f7-8f3c-51c7efd37b4a)

3. Open a **command prompt** in this folder. It is very important that the directory shown in the prompt is the one that holds the makefile.
4. input the following: **mingw32-make vs2022_clang MODERN_WIN_API=1 NOWERROR=1**

![image](https://github.com/user-attachments/assets/322209df-e5d1-4a4b-8d89-80cb2877a9b1)


**NOTE:** mingw32-make's **-j** flag allows you to specify the number of jobs to run in parallel in the make process, potentially speeding it up considerably. For my 12 core, 24 thread Ryzen 9900X I use **mingw32-make -j36 vs2022_clang MODERN_WIN_API=1 NOWERROR=1** .  Multiplying thread count by 1.5  (24 * 1.5 = 36) seems to work OK for me.

![image](https://github.com/user-attachments/assets/547c6bbd-0d86-4095-8e37-44b97dada597)


5. The Visual Studio 2022 solution (.sln) will be generated in the **(Mame Source Code folder)\build\projects\windows\mame\vs2022-clang** subdirectory.


## Step 4. Build the solution

1. Start Visual Studio 2022 and open the solution (.sln) file.
2. Ensure that the projects will be built with clang. Open a few random projects property pages and ensure **Platform Toolset** is set to **LLVM (clang-cl)**, for example:

![image](https://github.com/user-attachments/assets/3185f5ad-5d72-4cc6-9b9a-4060f8b12646)

3. Build the solution as normal. 
4. Don't expect the source to build entirely without errors. A few errors are acceptable. I've had to comment out code to get the build working a few times now.
5. Start debugging. HAVE FUN!
