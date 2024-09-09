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
    wget https://www.python.org/ftp/python/3.11.9/python-3.11.9-amd64.exe -OutFile c:\python-3.7.0.exe ; `
    Start-Process c:\python-3.7.0.exe -ArgumentList '/quiet InstallAllUsers=1 PrependPath=1' -Wait ; `
    Remove-Item c:\python-3.7.0.exe -Force
    
# Download Ninja to use as our build system for speedier builds. (It's bettern than MSVC. Trust me)
RUN mkdir C:\Ninja

RUN powershell.exe -Command `
    $ErrorActionPreference = 'Stop'; `
    wget https://github.com/ninja-build/ninja/releases/download/v1.10.0/ninja-win.zip -OutFile C:\Ninja\Ninja.zip ; `
    Expand-Archive C:\Ninja\Ninja.zip C:\Ninja ; `
    Remove-Item  C:\Ninja\Ninja.zip -Force


# Install GNuWin32 tools via Chocolatey for Lit testing
RUN powershell -Command `
	iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))

RUN choco install -y gnuwin32-coreutils.install
RUN choco install -y gnuwin
RUN choco install -y mingw
RUN choco install -y llvm



# # Install xz utility
# RUN powershell.exe -Command `
#     $ErrorActionPreference = 'Stop'; `
#     curl -s -L https://tukaani.org/xz/xz-5.2.5-windows.zip > C:\xz.zip ; `
#     Expand-Archive C:\xz.zip -DestinationPath C:\xz ; `
#     Remove-Item C:\xz.zip -Force ; `
#     setx PATH "%PATH%;C:\xz\xz-5.2.5-windows\bin"

# # Download and extract LLVM clang+llvm-18.1.8 into C:/clang
# RUN mkdir C:\clang

# RUN powershell.exe -Command `
#     $ErrorActionPreference = 'Stop'; `
#     curl -s -L https://github.com/llvm/llvm-project/releases/download/llvmorg-18.1.8/clang+llvm-18.1.8-x86_64-pc-windows-msvc.tar.xz > C:\clang\llvm.tar.xz -UseBasicParsing; `
#     tar -xf C:\clang\llvm.tar.xz -C C:\clang --strip-components=1 ; `
#     Remove-Item C:\clang\llvm.tar.xz -Force


RUN setx path "%path%;C:\Ninja"



ENTRYPOINT ["C:\\BuildTools\\Common7\\Tools\\VsDevCmd.bat", "-host_arch=amd64", "-arch=amd64", "&&", "powershell.exe", "-NoLogo", "-ExecutionPolicy", "Bypass"]