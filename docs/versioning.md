# Software Versioning

## Versioning Scheme (Semantic Versioning)
Versioning of DCAN Labs software should follow the [Semantic Versioning](https://semver.org/) standard, in which versions are assigned a version number **MAJOR.MINOR.PATCH**.  

In general:

**MAJOR** version is incremented when an update disrupts backward compatibility. (For a preprocessing pipeline this would entail reprocessing the data from scratch with the new version.)

**MINOR** version is updated when an update adds functionality in a backward-compatible manner.

**PATCH** version is updated when you make backward-compatible bug fixes.

**Documentation**

If the repository is available in GitHub, releases and tags should be named according to the Versioning Scheme. 

*(A “tag” is a property of a Git commit; a “release” is a GitHub property connected to a Git tag. For details see https://docs.github.com/en/repositories/releasing-projects-on-github/about-releases.)* 

Naming should be consistent for the Git tag, GitHub release and Dockerhub tag associated with any specific Git commit.

**In-software version identification**

Version of software should be reported in the following ways, whenever applicable:

For software run from command-line:
- In the output of the help command, or a version-reporting command
- At the beginning terminal standard output when the main function is run
- At the beginning of log and error files

**Release notes**

For each GitHub release, make a list detailing all changes made since the previous release within the same major version. (If the release begins a new major version, list the changes since the last release in the prior major version.) 

Changes should be categorized using the following shorthand codes adapted from [fMRIPrep’s release documentation](https://fmriprep.org/en/stable/changes.html):
  
<table>
  <tr>
   <td>
<strong>Code</strong>
   </td>
   <td><strong>Meaning</strong>
   </td>
  </tr>
  <tr>
   <td>BACKPORT  
   </td>
   <td>Adding a feature first introduced in a newer version into an old version
   </td>
  </tr>
  <tr>
   <td>CI
   </td>
   <td>Continuous integration
   </td>
  </tr>
  <tr>
   <td>DOCK
   </td>
   <td>Docker-related
   </td>
  </tr>
  <tr>
   <td>DOCU
   </td>
   <td>Documentation
   </td>
  </tr>
  <tr>
   <td>ENH
   </td>
   <td>Enhancement
   </td>
  </tr>
  <tr>
   <td>FIX
   </td>
   <td>Fixes a bug or issue
   </td>
  </tr>
  <tr>
   <td>MAIN
   </td>
   <td>Maintenance
   </td>
  </tr>
  <tr>
   <td>RF
   </td>
   <td>Refactoring (restructuring of code that does not change function input and output)
   </td>
  </tr>
</table>

Learn more about continuous integration [here](https://resources.github.com/ci-cd/)

## Release Categories

**Release Candidates**

Release Candidates (RCs) provisional builds intended for internal or limited testing to check if any critical problems have gone undetected into the code during the previous development period. These releases should be developed in a development branch and not the main branch on GitHub. In general, RCs should not be published publicly on Dockerhub or included in Zenodo. The “pre-release” flag in GitHub can be used to designate an RC. 

See **(link TBA)** for lab guidelines on code review and testing. 

**Stable releases**

An RC (release candidate) that has been through testing and code review. In the typical workflow, a [GitHub pull request](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests) to review and merge the changes from the development branch to the main branch, then a stable release can be created from the main branch.

 Stable releases should meet the requirements for the [NMIND Bronze coding standards certification](https://github.com/nmind/coding-standards-certification/tree/main/checklists)

**Long-term support releases (LTS)**

LTS releases are stable releases for which we commit to providing longer-term user support and issue fixes. 

When a new major release is made which breaks backward compatibility, consider designating a stable version from the prior major release line as an LTS release. This assures users who do not or cannot migrate to the new version that their current version will continue to be supported. 

*For DCAN computing team members: Bring LTS questions to Computing Team meeting to discuss whether LTS status should be designated for an older release*

**Deprecated releases**

Outdated releases where there won’t be continued development or support. Deprecated releases are older releases with no expectation of continued development or user support. These should be denoted in GitHub.
 
## GitHub webhooks

[GitHub webhooks](https://docs.github.com/en/developers/webhooks-and-events/webhooks/about-webhooks) are used to trigger an action in a connected website or application in response to an action on GitHub. Examples include [initiating a Docker image build on Dockerhub](https://docs.docker.com/docker-hub/webhooks/) or [updating a Zenodo entry](https://docs.github.com/en/repositories/archiving-a-github-repository/referencing-and-citing-content/) 
 
Webhooks require the GitHub user publishing the release to have repo admin permissions. If you plan on publishing releases on the [DCAN Labs GitHub](https://www.github.com/DCAN-Labs), reach out to the project manager. Project managers will give you admin privileges on the repo so that webhooks will execute.
 
## Software and source code on MSI 

- Stable version source code and images on MSI are stored on the **faird** share (<..>/faird/shared/code)
- When pulling Singularity images from Dockerhub to MSI, include release version in the filename e.g. **abcd-hcp-pipeline_v0.1.0.sif** 
- Pull specific tags rather than the default "latest" build, e.g. **singularity pull abcd-hcp-pipeline_v0_1_0.sif  docker://dcanumn/abcd-hcp-pipeline:v0.1.0**
- For non-versioned tags such as "unstable", append the date the tag was published to filename, e.g. **abcd-hcp-pipeline_unstable03152022.sif**  


