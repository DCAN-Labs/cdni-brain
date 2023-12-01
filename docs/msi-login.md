# Minnesota Supercomputing Institute (MSI) First Login Steps

To access the Minnesota Supercomputing Institute (MSI) system smoothly, confirm your eligibility, particularly if your tasks involve data analysis or processing. Visit  [Eligibility & Access Instructions](https://www.msi.umn.edu/content/eligibility-getting-access) for more detailed guidelines on eligibility and access requirements.

## Connecting to the Minnesota Supercomputing Institute (MSI) VPN

### VPN Installation:

To access MSI from outside the university area, you must first establish a secure connection using a Virtual Private Network (VPN).

- Find detailed VPN instructions on the [MSI VPN](https://it.umn.edu/services-technologies/virtual-private-network-vpn) page.
- Scroll halfway down the page to discover installers for Cisco AnyConnect on different operating systems.

### Cisco AnyConnect Installation:

- Download and install Cisco AnyConnect based on your operating system.
- Follow the installation prompts to complete the setup.

### VPN Connection Setup:

- Launch Cisco AnyConnect on your device.
- Log in using your credentials.
- During login, choose the "Split Tunnel VPN" option for a streamlined experience.

Once connected, you now have access to the Minnesota Supercomputing Institute resources.
By following these steps, you'll seamlessly connect to MSI, enabling you to analyze and process data efficiently.

## Registering for Duo Security

To enhance security when accessing online information, you must set up Duo Security. This added layer of security is mandatory for accessing UMN internal sites. Follow these [instructions](https://it.umn.edu/services-technologies/self-help-guides/duo-set-use-duo-security) to register and use Duo.

## Logging in to MSI

After connecting to MSI use the command `ssh -Y <USERNAME>@mesabi.msi.umn.edu` on [OnDemand](https://ondemand.msi.umn.edu/pun/sys/dashboard/batch_connect/sessions) Desktop terminal.

MSI can also be accesed through any regular terminal but you must first log in to a login node: `ssh -Y <user>@login.msi.umn.edu`. A login node can be used to browse, view files, etc.

When using a cluster to perform more advanced tasks, log in to one of MSI's clusters, like Mesabi or Mangi or if from a login node `ssh -Y <user>@mesabi or mangi`.

For additional guidance watch [MSI video tutorial](https://www.youtube.com/watch?v=PgD7WSI6CG4).

## Permissions

To ensure the data and code created can be accessed by all, update your `.bashrc` with the following steps (this only needs to be done the first time you access MSI). See [here](https://www.digitalocean.com/community/tutorials/bashrc-file-in-linux) for some info on what a `.bashrc` is.

Open your `.bashrc` file with a text editor, e.g. `emacs ~/.bashrc`.
Set umask to 0007. The umask is the default permission (self, group, like `faird`, and all users can be given read, write, and execute access) applied to the files you create. 0007 gives read, write, and execute access to you and all group members, but no one else (this could be anyone in the university). See here for details.
Close the file and type `source ~/.bashrc` into the terminal to apply the changes.
Your `.bashrc` is loaded each time you log in, so you only need to source it when you edit it mid-session. 

## Gaining Access
To gain access to MSI shares containing ABCD derivatives (`feczk001`, `miran045`, `rando149`, `smnelson`), you must be an approved collaborator on the UMN DUC.

* Create a [Login.gov account](https://www.login.gov/create-an-account/) 
* Use your login.gov credentials to [access the NDA](https://nda.nih.gov/user/login_required.html?originator=%2Fuser%2Fdashboard%2Fdashboard.html) - you’ll then be prompted to create an NDA account
* Email Borgne Raasch `braasch@umn.edu` your NDA username to be added to the next submission batch
* Outside collaborators should follow this link: ​​ [NDA - Adding Affiliated Institutions](https://docs.google.com/document/d/1w5BW14EHFSi4Lr1YDPm9CLTiy8JNdClt5KpKCOYnBH4/edit#heading=h.qdjbnp8qckwr)
* After Borgne confirms that you have been granted access to the DUC, email Russell Spafford `spaff010@umn.edu` to granted access to `feczk001`, `miran045`, `rando149`, or `smnelson` and let Borgne know if you also need access to the `NGDR`. For more information on what the `NGDR` is, see [here](https://rc.umn.edu/project/neuroimaging-and-genetic-data-resources). It is where we store our ABCC BIDS inputs and derivatives.

## Setting up your `.bashrc`

`.bashrc` is a Bash shell script that runs every time you start an interactive session and it lives in your home directory. In order to access your `.bashrc`, open a new terminal in an OOD session and run `cat .bashrc` to see what it currently is or run `emacs .bashrc` to open it in an editor if you need to make changes. 

Here is a template for what your `.bashrc` should look like

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
