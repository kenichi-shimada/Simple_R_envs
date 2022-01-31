# Simple_R_envs
A personal reference about a way to use a different version of R and libraries (without `renv` package or `conda`) for each project. This is particularly useful when coming back to re-run codes in older repos.

One can switch between different versions of R and a set of libraries for different projects.

First, switch to a specific version of R (which is assumed to be available here):
```sh
## Let's say you want to switch from 4.1 to 3.3:
cd  /Library/Frameworks/R.framework/Versions
ls -l Current #4.1
rm Current 
ln -s 3.3 Current 
```

Next, move to the top level of the project dir(`project_dir`) and create `libs_ver` library, set path in `.Renviron` file, also set `dir`, a home directory varaible, set in `.Rprofile` file that is loaded upon running R:
```sh
project_dir="/path/to/the/project/dir"
ver=`R --version | head -1 | cut -d' ' -f 3`
libs_ver=libs_$ver

cd "$project_dir" # the project directory manually set
if [ ! -d $libs_ver ]; then 
	mkdir $libs_ver
	echo R_LIBS_USER=\"$(pwd)/$libs_ver\" >> .Renviron
	echo "dir <- "\"$(pwd)\" >> .Rprofile
fi
```

Then run R from `$project_dir` every time you work on the project. Note you'll still load the libraries that are available system-wide. I'll have to turn that off explicitly next.
