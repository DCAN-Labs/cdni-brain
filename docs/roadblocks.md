# MSI Tips and Tricks

This page will detail some tips for working on MSI, common roadblocks encountered on MSI and how to fix them.

If you are experiencing errors/issues with general usage or Slurm, contact the MSI help desk (help@msi.umn.edu). If you're having issues that aren't detailed here, post a question in the `help-computing` channel on Slack.

## Workflow

Greg Conan developed a function that keeps track of all of the commands you use in a session and allows you to search for commands you've used previously. Add these functions to your .bashrc (located in your home directory):

```
  grephist() 
  
  { # Search through my most recent commands
    touch ~/.history.txt

    # Default values for input arguments
    to_show=10;   # -n
    to_check=50000; # -d

    search_for=(); # List of terms to search through command history for
    filter_out=(); # List of terms to exclude from search results

    search_fn="tail" # By default, show the NEWEST (MOST RECENT) commands

    while [ "${1}" != "" ]; do # Iterate over all input args
        case ${1} in
        -n | --lines) # Number of results to echo in output
            shift
            to_show=${1}
            ;;
        -d | --depth) # Number of commands in history to check
            shift
            to_check=${1}
            ;;
        -v | -x | --exclude) # Term to exclude from search results
            shift
            filter_out=(${filter_out[@]} "${1}");
            ;;
        -i | --initial | --inverted | --head) # Show OLDEST commands
            search_fn="head"
            ;;
        -h | --help)
            echo "Usage: grephist [-n lines] [-d depth] [SEARCH_TERM_1] [SEARCH_TERM_2]... -v [EXCLUDE_TERM_1] -v [EXCLUDE_TERM_2]...
Search through entire command history to find specific term(s) in past commands,
and then echo the most recent commands to use the term(s).
Options:
    -d, --depth:      Number of commands in command history to check. Default: ${to_check}
    -n, --lines:      Number of lines to echo in output. Default: ${to_show}
    -v, -x, --exclude:   Search term(s) to filter out.
    -i, --inverse, initial Include this flag to search OLDEST commands. Otherwise, will search NEWEST.
";
            return 1
            ;;
        *)
            search_for=(${search_for[@]} "${1}");
        esac
        shift
    done

    # Type validation: Ensure that -n and -d are integers
    for eachinput in $to_show $to_check; do
        if ! $(is_valid_whole_number $eachinput); then
            echo "Error: ${eachinput} is not a positive integer." >> /dev/stderr;
            return 1;
        fi
    done

    # Get contents of command-history text file
    search_cmd="${search_fn} $(fullpath ~)/.history.txt -n ${to_check}"

    # Filter for each search term
    for each_term in ${search_for[@]}; do
        search_cmd="${search_cmd} | grep ${each_term}";
    done

    # Filter out search terms
    for exclude_term in ${filter_out[@]}; do
        search_cmd="${search_cmd} | grep -v ${exclude_term}";
    done

    # Display the result
    search_cmd="${search_cmd} | ${search_fn} -n ${to_show}"
    eval ${search_cmd}
}

is_valid_whole_number() {
    [[ "${1}" =~ ^[0-9]+$ ]]; return ${?};
}

fullpath() {
    if [ $# -ne 0 ]; then
        for eacharg in "$@"; do
            if [ -f $eacharg ] || [ -d $eacharg ]; then
                fullpath_convert $eacharg
            else
                echo "Error: ${eacharg} not found." >> /dev/stderr;
            fi
        done
    else
        fullpath_convert ${PWD};
    #    echo Usage: fullpath \$some_relative_path
    fi
}

fullpath_convert() {
    inpath=${1};
    abs_inpath=$(realpath ${inpath});
    if [ "$(echo $abs_inpath | grep /panfs/ | grep /groups/)" = "" ]; then
        echo $abs_inpath
    else
        echo /home/${abs_inpath#*/panfs/*/groups/?/};
    fi
}

# Automatically save every command into a text file to save history
PROMPT_COMMAND='echo "$(date +"%Y-%m-%d %H:%M") $(history | tail -n 1)" >> ~/.history.txt'
```

Each share on MSI follows a similar directory structure:

```
|--home
   |--<share>
      |--shared
         |--code
         |--projects
         |--data
      |--<user_dir>
```

* You will have your own user_dir on each share which will be your x500. These are write protected directories, meaning only you have access to write to them. You have the ability to open them up, but it is not recommended. If you are working on something that other people might need access to, it is best to create a directory under `projects`. 

If you get a "not enough memory" error when trying to grab a srun, you must be ssh'd into mangi/mesabi/agate (`ssh -Y mangi/agate`)


## Disk Quota

If you get a `Disk quota exceeded` error:

* First check that the share isn't full: `groupquota -g share_name`

If the group isn't full, try changing the group of the file/directory:

* One file: `chgrp faird filename`

* All files in the current directory: `chgrp faird *`

* All files in a specific directory: `chgrp faird /home/faird/shared/*`

If the group is full:

* Check your usage for each share: `groupquota -u -g share_name`

* Check everyone usage for each share: `groupquota -a -g share_name`

* Post a message on slack alerting everyone that there is a quota issue. 

* If you know you have a large folder, consider moving data to the [s3](s3.md)

* If you are running into space issues but can't tell where they are coming from:

    * Open a new terminal and type `ls -al` to see all hidden files in your home directory 

    * Do you see a lot of ".proc-" files? Run `rm -r .proc-*` to delete all files that start with that prefix

    * Run `cd .apptainer/cache/blob/blobs` and clear out the `sha256` and `oci-tmp` folders (run `rm -rf *` inside the folders to delete all of their contents without deleting the folder itself)

## Access

If you cannot access MSI or it's running slow:

* MSI will be down all day on the first Wednesday of each month

    * MSI can also run slow during the week of maintenance day

* Check the status of MSI [here](https://status.msi.umn.edu/)

* Check the status of MSI nodes [here](https://download.genomics.umn.edu/website/slurmnodes/index.html)

If someone can't read/write/change your folders/files:

* Run `chmod gu+rwx path/to/file/or/dir -R` (`-R` only needed for directories)

* This will open up read, write, and execute permissions for the file/directory for the group and user (yourself)

If you need to change your active group:

* Run `newgrp <share>`
    
* This will let you make new files in the proper group

* Read more about this [here](https://opensource.com/article/19/9/linux-chgrp-and-newgrp-commands#:~:text=The%20newgrp%20command%20allows%20a%20user%20to%20override,all%20files%20must%20have%20the%20same%20group%20ownership.)

If you are getting FileNotFound errors in paths using `panfs/roc/`:

* MSI has moved the software library to a new location to migrate away from hardware that is being decommissioned

* These are the old paths and their new counterparts 

```
/soft	->  /common/software/install/migrated
/panfs/roc/msisoft	->  /common/software/install/migrated
/panfs/roc/soft/el6 ->  /common/software/install/migrated.softel6
/panfs/roc/intel	->  /common/software/install/migrated.intel
```

* Find out more information [here](https://www.msi.umn.edu/support/faq/how-do-i-update-my-workflow-after-software-library-migration)

## Applications

If you get this error when trying to use Firefox:
            
    "Firefox is already running, but is not responding. To use Firefox, you must first close the existing Firefox process, restart your device, or use a different profile."

There are two possible causes:

1. Another Firefox process is already running under your MSI profile (possibly in a separate MobaXTerm/NX/NICE session) in which case you must close that Firefox instance before launching a new one.

2. A previous Firefox process terminated abnormally (e.g. due to MSI session timing out while Firefox was running), leaving Firefox profile lock files in your ~/.mozilla directory which prevent Firefox from launching.   

There are two options to fix this:

* Run `firefox -P` and select default 

* Run this command to delete the lock files: 
    ```files=`find ~/.mozilla -name "*lock"`; for file in `echo $files`; do echo "removing $file..."; rm "$file"; done```

* If issue persists, try open the file with `chromium-browser html_file_to_open` 

If chromium fails to launch with a profile lock error (or fails to launch with no error message):

* Run this command to remove the lock files: `rm -rf ~/.config/chromium/Singleton*` 


For questions, suggestions, or to note any errors, post an issue on our [Github](https://github.com/DCAN-Labs/cdni-brain/issues).