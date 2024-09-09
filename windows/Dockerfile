# escape=`
FROM mcr.microsoft.com/dotnet/framework/sdk:4.8-windowsservercore-ltsc2019

# this is taken from https://github.com/justiceadamsUNI/LLVM-Windows-Dockerfile/blob/master/DOCKERFILE

# Setup admin user for certain LLVM build processes (Writing to pdb for example)
RUN net user /add llvm-admin
RUN net localgroup Administrators llvm-admin /add
USER llvm-admin

# Setup the shell.
SHELL ["cmd", "/S", "/C"]

# The following steps are modified from 
# https://docs.microsoft.com/en-us/visualstudio/install/build-tools-container?view=vs-2019&viewFallbackFrom=vs-2019
# Download the Build Tools bootstrapper.
ADD https://aka.ms/vs/16/release/vs_buildtools.exe C:\TEMP\vs_buildtools.exe

# Download and install Build Tools for Visual Studio 2019 for native desktop workload.
# This will be used to compile LLVM using the vc compiler.
# Note that Microsoft.VisualStudio.Workload.VCTools  includes CMAKE
RUN C:\TEMP\vs_buildtools.exe --quiet --wait --norestart --nocache `
    --installPath C:\BuildTools `
    --add Microsoft.VisualStudio.Workload.VCTools --add Microsoft.VisualStudio.Component.VC.ATLMFC --includeRecommended `
    || IF "%ERRORLEVEL%"=="3010" EXIT 0

# Install Python needed for lit testing	
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


ENTRYPOINT ["C:\\BuildTools\\Common7\\Tools\\VsDevCmd.bat", "-host_arch=amd64", "-arch=amd64", "&&", "powershell.exe", "-NoLogo", "-ExecutionPolicy", "Bypass"]