# escape=`
FROM mcr.microsoft.com/dotnet/framework/sdk:4.8-windowsservercore-ltsc2019

# Inspiration from https://github.com/justiceadamsUNI/LLVM-Windows-Dockerfile/blob/master/DOCKERFILE

# Setup admin user for certain LLVM build processes (Writing to pdb for example)
RUN net user /add llvm-admin
RUN net localgroup Administrators llvm-admin /add
USER llvm-admin

# Setup the shell.
SHELL ["cmd", "/S", "/C"]

RUN `
    # Download the Build Tools bootstrapper.
    curl -SL --output vs_buildtools.exe https://aka.ms/vs/16/release/vs_buildtools.exe `
    `
    # Install Build Tools with the Microsoft.VisualStudio.Workload.AzureBuildTools workload, excluding workloads and components with known issues.
    && (start /w vs_buildtools.exe --quiet --wait --norestart --nocache `
        --installPath "%ProgramFiles(x86)%\Microsoft Visual Studio\2019\BuildTools" `
        --add Microsoft.VisualStudio.Workload.AzureBuildTools `
        --remove Microsoft.VisualStudio.Component.Windows10SDK.10240 `
        --remove Microsoft.VisualStudio.Component.Windows10SDK.10586 `
        --remove Microsoft.VisualStudio.Component.Windows10SDK.14393 `
        --remove Microsoft.VisualStudio.Component.Windows81SDK `
        || IF "%ERRORLEVEL%"=="3010" EXIT 0) `
    `
    # Cleanup
    && del /q vs_buildtools.exe

# Install Python
RUN powershell.exe -Command `
    $ErrorActionPreference = 'Stop'; `
    wget https://www.python.org/ftp/python/3.7.0/python-3.7.0-amd64.exe -OutFile c:\python-3.7.0.exe ; `
    Start-Process c:\python-3.7.0.exe -ArgumentList '/quiet InstallAllUsers=1 PrependPath=1' -Wait ; `
    Remove-Item c:\python-3.7.0.exe -Force

# Download Ninja to use as our build system for speedier builds. (It's bettern than MSVC. Trust me)
RUN mkdir C:\Ninja

RUN powershell.exe -Command `
    $ErrorActionPreference = 'Stop'; `
    wget https://github.com/ninja-build/ninja/releases/download/v1.10.0/ninja-win.zip -OutFile C:\Ninja\Ninja.zip ; `
    Expand-Archive C:\Ninja\Ninja.zip C:\Ninja ; `
    Remove-Item  C:\Ninja\Ninja.zip -Force

# Install 7-zip for extracting large files
RUN powershell.exe -Command `
    $ErrorActionPreference = 'Stop'; `
    curl -s -L https://www.7-zip.org/a/7z1900-x64.msi >  C:\7zip.msi -UseBasicParsing; `
    Start-Process msiexec.exe -ArgumentList '/i C:\7zip.msi /quiet' -Wait ; 
    #Remove-Item C:\7zip.msi -Force

RUN mkdir C:\mingw

RUN powershell.exe -Command `
    $ErrorActionPreference = 'Stop'; `
    curl -s -L https://github.com/brechtsanders/winlibs_mingw/releases/download/12.4.0posix-12.0.0-ucrt-r1/winlibs-x86_64-posix-seh-gcc-12.4.0-mingw-w64ucrt-12.0.0-r1.zip > C:\mingw\mingw.zip -UseBasicParsing; `
    & 'C:\Program Files\7-Zip\7z.exe' x C:\mingw\mingw.zip -oC:\mingw ; `
    Remove-Item C:\mingw\mingw.zip -Force

# Add MinGW to the PATH
RUN setx PATH "%PATH%;C:\mingw\mingw64\bin"

# Install xz utility
RUN powershell.exe -Command `
    $ErrorActionPreference = 'Stop'; `
    curl -s -L https://tukaani.org/xz/xz-5.2.5-windows.zip > C:\xz.zip ; `
    Expand-Archive C:\xz.zip -DestinationPath C:\xz ; `
    Remove-Item C:\xz.zip -Force ; `
    setx PATH "%PATH%;C:\xz\xz-5.2.5-windows\bin"

# Download and extract LLVM clang+llvm-18.1.8 into C:/clang
RUN mkdir C:\clang

RUN powershell.exe -Command `
    $ErrorActionPreference = 'Stop'; `
    curl -s -L https://github.com/llvm/llvm-project/releases/download/llvmorg-18.1.8/clang+llvm-18.1.8-x86_64-pc-windows-msvc.tar.xz > C:\clang\llvm.tar.xz -UseBasicParsing; `
    tar -xf C:\clang\llvm.tar.xz -C C:\clang --strip-components=1 ; `
    Remove-Item C:\clang\llvm.tar.xz -Force


RUN setx path "%path%;C:\Ninja\;C:\clang\bin"



ENTRYPOINT ["C:\\BuildTools\\Common7\\Tools\\VsDevCmd.bat", "-host_arch=amd64", "-arch=amd64", "&&", "powershell.exe", "-NoLogo", "-ExecutionPolicy", "Bypass"]