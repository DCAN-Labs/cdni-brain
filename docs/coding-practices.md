# Development Recommendations

We at CDNI highly value creating code and workflows that are able to be reused and shared with other institutions. This means that we should be following best practices in order to make it easy for someone outside of our lab to use and understand our code. Below is some information about [NMIND](https://www.nature.com/articles/s41562-023-01647-0), which is a great place to start for exposure to best coding practices in the neuroimaging field and some common best practices within CDNI.

## NMIND Workflow

When code is developed without keeping best practices and standards in mind, challenges can arise when someone who didn't develop the code tries to use it. Some challenges that can come out of this is that the code if not fully developed/bullet-proofed, it has poor documentation, there are hard-coded paths, and it's overall not designed to be reused. To learn more about the NMinD programming recommendations, visit [this page.](https://hackmd.io/fJjIxGqWQB-wGIYZlnk5wA)

In an ideal world, code should be:

* Fully documented

* Include descriptions and examples

* Able to run on any platform

* Modularized, with modular elements able to be reused

* Containerized at its final stage

In order to try to fit into this ideal world, we try to follow the NMIND coding standard when developing code. When developing or reviewing code, the goal is to have the codebase [Bronze certified](https://github.com/nmind/standards-checklist/blob/98344089a6a1c4bcf46b4e7b6b7e5fafba880d87/checklists/bronze.md). 

A Bronze certification broadly entails the following:

* Documentation that is up to date 

* Documentation about usage, installation, dependecies, and expected outputs 

* Version control

* License and readme

* Issue tracking (typically through GitHub)

* Some sort of testing 

You can find all three levels of NMIND certifications [here.](https://github.com/nmind/standards-checklist/tree/98344089a6a1c4bcf46b4e7b6b7e5fafba880d87)

## License Recommendations for Software

As stated above, NMIND recommends including a license for your repository on GitHub. [Adding a license](https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions/adding-a-license-to-a-repository) is very simple and GitHub offers a large list of templates to choose from, but choosing which license is most appropriate can be a bit tricky.

For help with choosing a license, please review [Software Licensing at MIDB](https://docs.google.com/presentation/d/1gNhuHtjHY4E0biKdl5aXLN008UfeIu-e/edit?usp=sharing&ouid=113306241090115436408&rtpof=true&sd=true) presented to our group by Sandipan Banergee, Ph.D. and Eric Olson from the UMN Tech Transfer Office (Aug 2024). This provides an extensive overview of licensing and open source. For our purposes, as we typically opt for open source, they generally recommend using the UMN source available license for non-commercial use. To summarize, this allows for wider adoption among researchers and prevents 3rd parties from commercializing without giving anything back to the creators, UMN, your department/school/institute, etc. They also note that, "some OS licenses have explicit patent rights, and experts argue that all OS licenses have implicit patent rights—So cannot co-exist happily with patents since the intent is opposite" (slide 29). 

The powerpoint also includes information on services they provide in the event that you *do* wish to commercialize or see potential for commercialization down the road, mechanisms of which include license technology, spin out company, online express licensing, and sponsored research.

**Highlight from their website [UMN Tech Transfer: Releasing Open Source](https://research.umn.edu/units/techcomm/university-inventors/releasing-open-source)**

> **License Recommendations for Software**
>
> **Preferred**: [GNU General Public License version 3](https://www.gnu.org/licenses/gpl-3.0.en.html)
> 
>  - See [TR;DL Legal](https://tldrlegal.com/) for a summary of the GPL v3.0 license
>
> **Acceptable:** [MIT License](https://opensource.org/licenses/MIT), [BSD License](https://opensource.org/licenses/BSD-3-Clause), [Apache 2.0 License](https://opensource.org/licenses/Apache-2.0), [LGPL v3.0 License](https://opensource.org/licenses/LGPL-3.0)
> 
> - See TR;DL Legal for a summary of the:
> [MIT License](https://tldrlegal.com/license/mit-license)
> [BSD License](https://tldrlegal.com/license/bsd-3-clause-license-(revised))
> [Apache 2.0 License](https://tldrlegal.com/license/apache-license-2.0-(apache-2.0))
> [LGPL v3.0 License](https://tldrlegal.com/license/gnu-lesser-general-public-license-v3-(lgpl-3))

[TR;DL Legal](https://www.tldrlegal.com/) is a site that breaks down differences between license as simply as possible

**Additional resources:**
- [Express License Self-Service Licensing Platform](https://drive.google.com/file/d/1ycKBGFGf-H9BFDdM3k1tfh_XMCOdGtYV/view)
- https://choosealicense.com/licenses/
- [A Data Scientist's Guide to Open Source Licensing](https://medium.com/@mattmecoli/a-data-scientists-guide-to-open-source-licensing-c70d5fe42079)


## Record Keeping

**Record Change Dates**
- The following is an example of how to do this:

```
author: Audrey Houghton
created on: 11/25/2023
last modified: 11/30/2023
by whom: Audrey Houghton
```

**Commit Often**
- Make sure that you are regularly pushing your changes to a branch on GitHub with concise but descriptive commit messages.
- Don't necessarily have to wait until you're done with a task to push changes, just mark where you are.
- You can commit single files at a time so you don't have to write one message to describe many file changes.

**Error / Out Logging**
- The more descriptive you can make your codebase's logs, the better.
- Start and end time logs can be helpful, as well as some sort of "Finished Successfully" message.
- Jacob Lunquist and Greg Conan have developed an error query script that will search through `.err` files for specified errors. See more info on [this page](error_query.md)

**Commenting**
- At the top of each script within a given repository, describe what that script is doing.
- At the beginning of each function, define the purpose of the function, what its inputs are, and what its outputs are.
- Comment your code as you develop it, this is easier than going back to old code and trying to figure out what is going on.
- Make a note of any hard coded variables and why they are hardcoded.

Here is an example of a well commented (python) function:

```
def add_session_id(input_file, output_file, session_id):
    """
    Input: txt file of subjectIDs (with or without the "sub-" prefix), output file, and the session ID 
    Adds the session ID to the end of each line of the input file
    """
    with open(input_file, 'r') as infile:
        lines = infile.readlines()

    # Add session ID to each line
    modified_lines = [f"{line.strip()},{session_id}\n" for line in lines]

    with open(output_file, 'w') as outfile:
        outfile.writelines(modified_lines)

    print(f'Data written to {output_file}')
```


## Obscuring Filepaths

It's generally considered best practice to not share full filesystem paths on public GitHub repositories. For repositories that have file paths in the code, we therefore recommend using a config.json file when necessary to obscure file paths from your local computer, MSI, etfc:

Within your local repository, create a `config.json` file that contains, for example, a tier1 path you don’t want to make available on GitHub:

```
{
"tier1_path": "/your/file/path"
}
```

Create a file called `.gitignore` and add `config.json` so that you or other users don't accidentally publish their local config.json to the public repository (this can of course include other files/folders to ignore, such as logs):

```
# ignore config.json
config.json
```

Grab the path within your python script using the json module:

```
import json

with open("config.json") as json_data_file:
    data = json.load(json_data_file)
tier1_path = data['tier1_path']
```

This way, instead of having the file path visible in the code, you are pulling it from a config file that isn't public for others to see.

For questions, suggestions, or to note any errors, post an issue on our [Github](https://github.com/DCAN-Labs/cdni-brain/issues).
