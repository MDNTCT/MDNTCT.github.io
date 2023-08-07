

# Beginners' Guide To Factoring

This is a guide for Windows users who are interested in factoring.

I will introduce some softwares for factoring and also how to use them.

## 1.Downloads

You need to download all the software you need to get started.

This is the recommended site:https://download.mersenne.ca/

**Necessary programs : msieve , GGNFS , GMP-ECM , YAFU**

You need to go into the msieve , GGNFS , GMP-ECM , YAFU (version 1.34 is the most stable one) folder in this website and download the latest binaries. (Also unzip them)

Besides , you also need to download and install Python 3 (the recommended version).

**Optional : CADO-NFS**

If you want to use CADO-NFS , you should also download WSL2 , which provides a Linux-like environment . (Systems requirements can be found on Microsoft website)

Now the downloaded folders are : ggnfs , msieve , yafu-1.34 , gmp-ecm.

## 2.Preparations&Usage

I assume you have downloaded all the software mentioned above.

Create a folder called " factor " . (To me it's E:\factor.)

Extract all the downloaded files into the factor folder.

Rename the ggnfs folder "ggnfs-bin"

Copy the folder ggnfs-bin folder and rename it as " GGNFS " . They should have the same files inside.

The factor folder should look like this: (Change your folder names exactly the same if they are different from the followings)

```
GGNFS
ggnfs-bin
msieve
yafu-1.34
gmp-ecm
```

### Ⅰ.GGNFS Preparations

Copy all the files in the msieve directory and paste them into the GGNFS folder.

The GGNFS folder should look like this:

```
def-nm-params.txt
def-par.txt
factLat.pl
factMsieve.pl
factmsieve.py
ggnfs-doc.pdf
gnfs-lasieve4I11e.exe
gnfs-lasieve4I12e.exe
gnfs-lasieve4I13e.exe
gnfs-lasieve4I14e.exe
gnfs-lasieve4I15e.exe
gnfs-lasieve4I16e.exe
makefb.exe
matbuild.exe
matprune.exe
matsolve.exe
msieve.exe
pol51m0b.exe
pol51m0n.exe
pol51opt.exe
polyselect.exe
procrels.exe
sieve.exe
sqrt.exe
```

*If you cannot find " factmsieve.py " in the downloaded GGNFS folder , you can download it here :http://gladman.me.uk/computing/factmsieve.py*

Open the factmsieve.py and edit it:

Change lines 63-64 from:

```
# Set binary directory paths
GGNFS_PATH = 'C:/Users/brg\Documents/Visual Studio 2015/Projects/ggnfs/bin/x64/Release'
MSIEVE_PATH = 'C:/Users/brg/Documents/Visual Studio 2015/Projects/msieve/bin/x64/Release'
```

to:

```
# Set binary directory paths
GGNFS_PATH = '.'
MSIEVE_PATH = '.'
```

Change line 67 from:

```
NUM_CPUS = 4
```

to whatever the # of CPUs you want to use for the factoring. 

Comment out the following lines:

```
>>>>> code commented out - Part I
# # Figure the time scale for this machine.
# output('-> Computing time scale for this machine...')
# (ret, res) = run_exe(PROCRELS, '-speedtest', out_file = subprocess.PIPE)
# if res:
# tmp = grep_l('timeunit:', res)
# timescale = float(re.sub('timeunit:\s*', '', tmp[0]))
# else:
# timescale = 0.0

>>>>> code commented out - Part II
#print('Scaled time: {0:1.2f} units (timescale= {1:1.3f}).'
# .format(total_time * timescale, timescale))
```

Save the changes.

Now you have finished with GGNFS preparations.

#### GGNFS usage

To use GGNFS to factor numbers,simply create a file in the GGNFS folder called "number.n"(or whatever name you like) with the format

```
n:<a number>

```

There must be an empty line at the end of it.

Then cd to the GGNFS directory and type

```
python factmsieve,py number.n
```

And you can enjoy factorization.

### Ⅱ.YAFU preparations

Open the yafu.ini file in the yafu-1.34 folder.

Change it to look like this and save the changes:

```
B1pm1=100000
B1pp1=20000
B1ecm=11000
rhomax=1000
threads=<the number of threads you want yafu to use>
pretest_ratio=0.25
ggnfs_dir=..\ggnfs-bin\
%ggnfs_dir=/ggnfs-bin/
ecm_path=..\gmp-ecm\ecm.exe
%ecm_path=../ecm/current/ecm
```

Finished.

#### YAFU usage

cd to your yafu-1.34 folder and type:

```
.\yafu-x64.exe "factor(a number)"
```

If you want to use GGNFS with YAFU(which has been filled in the ini file),type

```
.\yafu-x64.exe "nfs(a number)"
```

The number can be an expression,like

```
(10^97-1)/9
```

or

```
2^499-1
```

Then yafu will factor the number itself.

The preparations and usage information ends here.

## 3.Optional Software

If you want to use CADO-NFS,which is much faster than GGNFS on Windows,you need to use WSL. It's maybe the fastest and easiest way to use an Linux app on Windows. 

You need to install GMP first.

Open WSL and enter

```
sudo apt-get update
```

Enter your password,then enter

```
sudo apt-get install g++ m4 zlib1g-dev make p7zip
```

Download GMP from https://gmplib.org/download/gmp/gmp-6.2.1.tar.xz

Rename the gmp-6.x.x folder to gmp.

Extract to the factor folder.

Move into the GMP folder in WSL.

```
cd /mnt/.../factor/gmp
```

type

```
./configure
```

```
make
```

```
make check
```

If all checked out, type:

```
sudo make install
```

Now you have installled GMP.



Stay in the factor folder,enter

```
git clone https://gitlab.inria.fr/cado-nfs/cado-nfs.git
cd cado-nfs
make
```

After it finishes,enter

```
./cado-nfs.py 90377629292003121684002147101760858109247336549001090677693
```

to check if it works alright. Wait a few seconds,and something like this should appear

```
Info:root: Using default parameter file ./parameters/factor/params.c60
Info:root: No database exists yet
Info:root: Created temporary directory /tmp/cado.1lafrwdi
Info:Database: Opened connection to database /tmp/cado.1lafrwdi/c60.db
Info:root: Set tasks.threads=8 based on detected logical cpus
Info:root: tasks.polyselect.threads = 2
Info:root: tasks.sieve.las.threads = 2
...
Info:Quadratic Characters: Total cpu/real time for characters: 0.16/0.0801561
Info:Square Root: Total cpu/real time for sqrt: 2.07/0.413262
Info:HTTP server: Shutting down HTTP server
Info:Complete Factorization: Total cpu/elapsed time for entire factorization: 47.08/38.8295
Info:root: Cleaning up computation data in /tmp/cado.1lafrwdi
760926063870977 773951836515617 260938498861057 588120598053661
```

Other usage on CADO-NFS can be found in the README file.

## 4.Usage examples

### YAFU:

```
.\yafu-x64.exe "factor(44631589825541155210729717604799021190914952109987)"
```

```
.\yafu-x64.exe "siqs(rsa(200))"
```

```
.\yafu-x64.exe "nfs(2^401-1)"
```

```
.\yafu-x64.exe "ecm(44631589825541155210729717604799021190914952109987,200)" -B1ecm=1e6
```

### GGNFS:

```
python factmsieve.py number.n
```

inside number.n:

```
n:446315898255411552107297176047990211909149521099871234567817662781

```

### CADO-NFS:

```
./cado-nfs.py 446315898255411559825541155210729717601136643651124567789981
```

## 5.Factoring Websites

The biggest factoring website:

[factordb.com](http://factordb.com/)

The best factoring forum:

[mersenneforum.org](https://www.mersenneforum.org/index.php)

Use ECM online:

[Integer factorization calculator](https://www.alpertron.com.ar/ECM.HTM)

## 6.A Shortcut

For those who meet problems,I have uploaded my version of the factor folder to Github as zip. You can download directly from https://github.com/MDNTCT/Factoring/blob/main/factor.zip .

The only changes required are the following:

```
yafu.ini------threads
factmsieve.py------cores and number of CPU
```

The package does not contain CADO-NFS,so you need to install it yourself.

## 7.Other

All information on usage can be found in the README files in YAFU,CADO-NFS and GGNFS folders.

**References:**

https://mersenneforum.org/showthread.php?t=23078&page=3

http://mersenneforum.org/showthread.php?t=23079

https://mersenneforum.org/showthread.php?t=23089

https://gilchrist.great-site.net/jeff/factoring/nfs_beginners_guide.html
