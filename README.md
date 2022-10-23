# bgVenv
- run python script in venv in background on linux
- original approach from [*run Python in venv in background (MacOS)* tutorial](https://arshovon.com/blog/python-background/)

## Installation (Raspberry Pi: Ubuntu Desktop 22.04.1 LTS (64-bit); not tested)
> not required to run script in background "in production" (see *Run Script in Background (Raspberry Pi Terminal)* below)

1. clone repo
2. enter repo folder: `cd bgVenv`
3. create virtual environment: `python3 -m venv venv`
4. activate virtual environment: `source venv/bin/activate`
5. update pip: `python3 -m pip install --upgrade pip`
6. install requirements: `pip install -r requirements.txt`
7. run program as described below (and in [tutorial](https://arshovon.com/blog/python-background/))

## Run Script in Background (Raspberry Pi Terminal)
> - on Ubuntu Desktop 22.04.1 LTS (64-bit)
> - accessed Raspi from Windows cmder: `ssh pi@192.168.177.22`
>   - py script continues running after closing terminal (cmder) on windows, creating new terminal & shh-ing into Raspi again - until stopped by `kill` command with (see **example stop** below)

- based on tutorial: [*run Python in venv in background (MacOS)*](https://arshovon.com/blog/python-background/)
  - run python script (here: `background_script.py`) in virtual environment in background, using shell script (`run_bg.sh`), which activates venv and executes python script by the help of `nohup`(!)
    - use `python3` instead of `python` in `run_bg.sh`
    - [make shell script (`run_bg.sh`) executable](https://askubuntu.com/a/396655): `chmod +x ./run_bg.sh`
### example run
> can also be executed without venv activated (because `run_bg.sh`) activates venv
```
(venv) pi@pi-desktop:~/Dokumente/bgVenv$ nohup ./run_bg.sh > custom-output.log &
[1] 25482
(venv) pi@pi-desktop:~/Dokumente/bgVenv$ nohup: ignoring input and redirecting stderr to stdout
ps aux | grep background_script.py
pi         25483  8.5  0.7 189880 28984 pts/0    Sl   20:55   0:00 python3 -u background_script.py
pi         25496  0.0  0.0   9008  2000 pts/0    S+   20:55   0:00 grep --color=auto background_script.py
```
- creates `custom-output.log` which contains output which would usually be printed to command line
### example stop
```
(venv) pi@pi-desktop:~/Dokumente/bgVenv$ ps aux | grep background_script.py
pi         25483  0.2  0.7 189880 30060 pts/0    Sl   20:55   0:00 python3 -u background_script.py
pi         25575  0.0  0.0   9008  2000 pts/0    S+   20:59   0:00 grep --color=auto background_script.py
(venv) pi@pi-desktop:~/Dokumente/bgVenv$ kill 25483
(venv) pi@pi-desktop:~/Dokumente/bgVenv$ ps aux | grep background_script.py
pi         25589  0.0  0.0   9008  2000 pts/0    S+   20:59   0:00 grep --color=auto background_script.py
[1]+  Exit 143                nohup ./run_bg.sh > custom-output.log
(venv) pi@pi-desktop:~/Dokumente/bgVenv$
```
