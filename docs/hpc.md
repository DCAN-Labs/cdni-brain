# Interactive High Performance Computing (HPC) Resources 

Read: [Interactive HPC @ MSI](https://www.msi.umn.edu/content/interactive-hpc)


MSI offers several options to remotely access HPC resources. These will allow you to access stored data, run processing jobs, and other computing heavy tasks. **Note:** The first Wednesday of every month, MSI will be down for maintenance. Sometimes this spills over and can effect MSI performance for the whole week. 

* **Open OnDemand**: A web portal to access MSI's compute clusters in your browser, which can be used for accessing MSI files and running jobs.
    * Select *Interactive Apps* at the top and select which Desktop you would like
    * The *Persistent Desktop* will run until next maintenance day 
    * If selecting *Desktop* choose which cluster, how many resources, and the time you need 
    * Agate is good for visualization and GPU utilization
    * Mesabi is good for running long jobs 
    * Several other apps are available, including MATLAB and RStudio Server

![Open OnDemand Window](img/ood_example.jpeg)

* **Jupyter Notebooks:** A web portal for notebook-based computing in the browser, which can be used for reproducible and shareable data analysis, visualization, and scripted control of larger tasks. Currently supports Python 2, Python 3, and R. Offered on OOD as a server.
    * Great for playing around with a database initially
    * Formalizing code here is not recommended
* **Terminal:** User input might come through a command line interface (i.e., system shell) while debugging a piece of software 
    * Great for working with codebases on VScode
* **srun:**  Submit an interactive job on the current HPC system (OOD or Terminal) to provide a command line or graphical interface to interactively run software.
    * Needed for running programs / visualizing data for more than 15 minutes on any system listed above

## Setting up your .bashrc

.bashrc is a Bash shell script that runs every time you start an interactive session and it lives in your home directory. In order to access your .bashrc, open a new terminal in an OOD session and run `cat .bashrc` to see what it currently is or run `emacs .bashrc` to open it in an editor if you need to make changes. 

Here is a template for what your .bashrc should look like

            # .bashrc startup script for login shells
            #

            # Set your umask.
            # umask 077       # -- private, only you have access to your files
            # umask 022     # -- anyone can read and execute your files
            # umask 027     # -- only members of your group can read/execute your files
            umask 007    # -- created files may take upto read/write/execute

            # Set the prompt.
            PS1="\u@\h [\w] % "

            # Add your aliases here.
            # alias s='ssh -X'

            # Set your environment variables here.
            # export VISUAL=vim

            # Uncomment the if statement below to enable bash completion.
            # if [ -f /etc/bash_completion ]; then
            #  source /etc/bash_completion
            # fi

            # Load modules here
            # module load

            export PATH=/home/dhp/public/storage/s3policy_bin/:$PATH

            grephist() { # Search through my most recent commands
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

The three things you'll most likely need change from your original .bashrc are

- Add `umask 007` to allow all the files you create to automatically give the group and user read/write/execute permissions

- Add `export PATH=/home/dhp/public/storage/s3policy_bin/:$PATH` to be able to set s3 policies, which is neccessary for adding people to your s3 bucket (more information [here](s3.md))

- Add the `grephist()` function, which will allow you to see all of your previous commands you've made. The MSI history function will only show the commands you've made from that session but this command saves every command you've written since you've added this function. Below is the usage again to make it easier to find and read

            Usage: grephist [-n lines] [-d depth] [SEARCH_TERM_1] [SEARCH_TERM_2]... -v [EXCLUDE_TERM_1] -v [EXCLUDE_TERM_2]...
            Search through entire command history to find specific term(s) in past commands,
            and then echo the most recent commands to use the term(s).
            Options:
                -d, --depth:      Number of commands in command history to check. Default: ${to_check}
                -n, --lines:      Number of lines to echo in output. Default: ${to_show}
                -v, -x, --exclude:   Search term(s) to filter out.
                -i, --inverse, initial Include this flag to search OLDEST commands. Otherwise, will search NEWEST.
