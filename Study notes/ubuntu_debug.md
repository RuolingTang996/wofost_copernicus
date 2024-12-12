# Modifying the Development.c for Chinese cabbage

## Clone Git Repository from Github
- Open VScode, in the Welcome page, select the "Glone Git Repository" (makes sure Git is installed), http://github.cm/RuolingTang996/wofost_copernicus
- Enter this folder, and check if the folder and files are complete

## Install Linux system (WLS) on Windows, which succesfully runs "make clean" and "make" commends 
- Install the Ubuntu on your wur lab                                                    ! You might need to call the IT Service to get the admin permission
- In the terminal of VSCode, you need to add one WLS terminal and commend there         ! you can also directly code on Ubuntu window
- Install some required packages in Ubuntu window or via the VSCode terminal (WSL)      ! below codes are from Ignacio:

### Update and install python
`sudo apt update`
`sudo apt install python3 python3-pip`

### GCC (GNU Compiler Collection)
check gcc is installed: `gcc --help`
Or install: `sudo apt install build-essential`

### MPI (not used at the moment as MPI debug is not straightforward)
`sudo apt-get install mpich libmpich-dev`

### JSON-C library
`sudo apt install libjson-c-dev`

### Udunits library
`sudo apt-add-repository universe`
`sudo apt-get install libudunits2-dev`

### To debug:
`sudo apt install gdb`

## Compilar the sub-module files into one wofost
- make clean
- make

## Debugging
-  Set suitable envriroment (not Win32) (should be WSL:Ubuntu) for debugging
-  Write the launch.json file                                                           ! I modified based on Yixuan's version
-  If still failed to debug, Read the DEBUG CONSOLE error and ask help from Iwan/Karun

### Debug Wofost on wur; launch.json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Debug Wofost on wur",
            "type": "cppdbg",
            "request": "launch",
            "program": "${workspaceFolder}/wofost",  
            "args": [],                                // Any arguments you want to pass to your program
            "stopAtEntry": false,
            "cwd": "${workspaceFolder}",               // Current working directory
            "environment": [],
            "externalConsole": false,                   // Optional: Opens an external console
            "MIMode": "gdb",
            "setupCommands": [
                {
                    "description": "Enable pretty-printing for gdb",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ],
            "miDebuggerPath": "/usr/bin/gdb"           // Path to gdb (verify it's correct for your system)
        }
    ]
}

## Adding the CRPTYPE = 0,1,2 in this WOFOST_COPERNCUS
### crop.h                                                                              !Add the CRPTYPE
### Develop.c                                                                           !Add two viod
### extern.h                                                                            !Add the DevelopmentRate_Cabbage(); ProcessDevelopmentRate();
### Filvar.c                                                                            !Add variable [66]
### wofost.h                                                                            !67

## Push it back to Github (Manually storaged each time after change and upload to Github)
- git init                                                                              ! 初始化 Git 仓库
- git remote -v                                                                         ! 确保当前目录的远程仓库已经正确配置
- git remote add origin https://github.com/RuolingTang996/wofost_c                      ! 确保我是配置好特定的repository的
- git add .                                                                             ! add所有更改和变动
- git add data/Forcing/historical/GFDL-ESM4/*.                                          ! add特定的更改/变动
- git commit -m "Add GFDL-ESM4 data to Forcing"                                         ! 提交更改
- git branch -M master                                                                  ! 
- git remote add origin https://github.com/RuolingTang996/wofost_c                      ! 确保我是配置好特定的repository的
- git push -u origin master                                                             ! PUSH back
- git status                                                                            ! Check status
