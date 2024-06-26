# Best Practices

We at DCAN Labs highly value creating code and workflows that are able to be reused and shared with other institutions. This means that we should be following best practices in order to make it easy for someone outside of our lab to use and understand our code. Below is some information about [NMIND](https://www.nature.com/articles/s41562-023-01647-0), which is a great place to start for exposure to best coding practices in the neuroimaging field. Below are some common best practices within DCAN labs.

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


## Logging Codebase Changes

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

## Error / Out Logging
- The more descriptive you can make your codebase's logs, the better.

## Commenting
- At the top of each script within a given repository, describe what that script is doing.
- At the beginning of each function, define the purpose of the function, what its inputs are, and what its outputs are.

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