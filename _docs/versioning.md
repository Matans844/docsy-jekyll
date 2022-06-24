---
title: Versioning your Docs
description: How to turn on and use versioning
---

# Versioning

Versioning documentation is a useful feature that can be achieved using Docsy Jekyll. It allows you to take snapshots of your documentation that may align with a specific version of a product or service, or you may wish to just keep a historical view of the changes that have been made to your documents over time.

{% include alert.html type="info" title="Note: This only allows you to version documents inside the _docs folder, it does not allow you to version any files outside of that, such as Pages, Posts etc." %}


## Implementation

The way in which versioning is achieved is by duplicating your whole document directory and placing it inside an 'archive' subdirectory with the name of that version. The following illustrates what the base document root looks like without another version for this Docsy Jekyll site.

```
document root
│      
└─── _data
└─── _docs
        └─── extras
        └─── subfolder
        example-page.md
        getting-started.md
        subfolder.md
        versioning.md
```
To create a version of this documentation we first create a folder with the version name we want to use inside of the 'archive' folder and copy all the files and folders from our root _docs folder into that new folder, we therefore end up with a structure such as below where we moved the older documentation into the folder 'Previous'. Once we have done that we effectively have taken a snapshot of our documents at a point in time and will no longer maintain these. If you look at the structure of this site you will see the 'Previous' folder, which now holds our previous documentation, does not contain the versioning.md markdown file as this is new in the current release.

<pre><code>
document root
│      
└─── _data
└─── _docs
        └─── extras
        └─── subfolder
        └─── Archive
              └─── <span style="color:blue;"><b>Previous</b></span>
                  └─── extras
                  └─── subfolder
                  example-page.md
                  getting-started.md
                  subfolder.md
        example-page.md
        getting-started.md
        subfolder.md
        versioning.md
</code></pre>

You would now continue to add, update or remove markdown files from the main _docs directory in order to work on the current version of your documentation. 

Once the versioning has been correctly configured and you have created a snapshot, you can access those documents by adding the version to the url to access that archived version (e.g. /docs/Archive/Previous/getting-started). 

To enable the users to interact with these versioned documents through the UI there needs to be some updates to the _config.yml file to setup and enable it. The following is a snippet from the _config.yml file specific to the versioning options that are available.

```yml
version_params:
  version_menu: "Release"
  version_dir: Archive
  versioning: true
  latest: current
  versions:
    - main
    - current
    - beta
    - v3.1
    - v3.0.1
    - v3.0
    - v2.4.5
    - v2.4.4
```

### Versioning Options


| Parameter | Description | Values |
| --------- | ----------- | ------ | 
| versioning | This determines whether the versioning functionality is enabled | true \| false 
| version_menu | In addition to the version dropdown you can specify some text to display alongside it<br/> If you do not wish to use this then make this value an empty string | Release
| version_dir | This is the directory where all the alternative versions of the documentation will be contained beneath the _docs directory <br/>i.e. setting this to the value of Archive (Note: **No** trailing slash) will mean all alternative versions will be found in _docs/Archive/ | Archive 
| latest | From the list of versions this determines the one which relates to your base docs directory.<br/> Note the url for your docs will not contain a version identifier for this particular version| current \| v3.1
| versions | A list of versions you wish to display in the version dropdown <br/> Note that there are several special keywords that can be used: current, main, alpha, beta, rc and pre, see below for more details  | current \| v1.0 \| v2.1.3

{% include alert.html type="warning" title="Note: The versions listed must have the same name as the folder name in which that version of the documentation resides otherwise jekyll it will not find it! The exception is the version 'current' which does not have a separate folder and is used to inform jekyll to utilise the base _docs folder." %}

### Version Keywords
There are six special keywords that can be used in the versions config.
* main
* alpha
* beta
* rc
* pre
* *current*

The value **main**, is used to display the latest development version of the documentation, a banner is used to denote that the code and documentation maybe unstable and that they are not guaranteed to be correct or up to date and could change at any time.

The value of **alpha**, **beta**, **rc** and **pre** are used to denote that the documentation is a pre-production version and as such a banner is displayed announcing that the code/documentation maybe unstable and the content in the pages are still work in progress so may not be completely up to date or correct.

If you wish to utilise this feature then you must create folders within your _docs/*Archive* directory with the names of those you wish to use and place your documentation inside it/them.

The value of **current** is used to align with the latest version of your documentation in your base _docs folder so there **doesn't** need to be a specific folder created and named 'current' to serve them. The name 'current', for the value of 'latest' in the previous example, can be changed to whatever you wish to name the current release but be aware that the version set in the _config.yml file also needs to match it! See the example below where we have called the latest release v3.2 rather than 'current' and we also have a version exactly named v3.2 too, as mentioned this version will point to your base _docs directory. 

```yml
version_params:
  version_menu: "Release"
  versioning: true
  latest: v3.2
  versions:
    - main
    - v3.2
    - beta
    - v3.1
```

## Table of Contents Handling (TOC)

As with the documents, your TOC contents also need to be updated so they correctly point to the document versions you are viewing.

You will need to update the toc file as the structure of your documentation/toc changes over time but as we have taken a snapshot of our documentation we need to do the same thing for our toc so it reflects the document structure at that point in time too. You will see that for this site we have the versions of Current and Previous and you will see that the Current version has all the information about versioning which didn't exist in the prior version.

To enable the TOC to change based on the version, we have to create a new toc file and name it {version}-toc.yml, so in this site we have created previous-toc.yml in the _data directory that has the TOC structure for the prior version where we did not have the versioning.md file present. 


{% include alert.html type="info" title="Note: The toc.yml file will always be our current table of contents pointing to the most recent version of the documentation in the base of our _docs folder. When adding additional items to your toc.yml file you do not need to worry about the version identifier as that is taken care of by Docsy Jekyll when viewing alternative versions, continue to have the urls pointing to the files in the base docs directory." %}

Here is what our current toc.yml file in our _data directory looks like, the previous-toc.yml file will look identical apart from omitting the 'Versioning' title and link which was added for this release.

```yml
title: Documentation
  url: docs
  links:
    - title: "Getting Started"
      url: "docs/getting-started"
      children:
        - title: Features
          url: "docs/getting-started#getting-started"
        - title: Development
          url: "docs/getting-started#development"
        - title: Customization
          url: "docs/getting-started#customization"
    - title: Versioning
      url: "docs/versioning"
    - title: "About"
      url: "about"
    - title: "News"
      url: "news"
- title: "Extras"
  url: "docs/extras"
  links:
    - title: Quizzes
      url: "docs/extras/example-quiz"
    - title: Tags Page
      url: "tags"
```

Now not every version of your documentation will have a change in structure and therefore require an update to toc.yml. To remove this overhead a mapping file is used to tell Docsy Jekyll which TOC it should use for a particular version,
the benefit of this is that if the structure of your site does not change then you do not need to create an additional toc.yml file.

You will have a toc-mapping.yml file in your _data directory and the contents will contain the code below to map a particular version with a particular TOC file to use. You need to update this file for each version you create even if you do not need to create a new TOC file.

```yml
# This file can be used to explicitly map a release to a specific table-of-contents
# (TOC). You'll want to use this after any revamps to information architecture, to ensure
# that the navigation for older versions still work.

Current: toc
Previous: previous-toc
```
{% include alert.html type="info" title="Note: If a version cannot be found in this mapping file then the standard toc.yml in the _data directory will be used instead." %}

Because we added the versioning.md markdown file to this site we needed a new toc as well so that it could be accessed from the menu. If we had only made an update to the content of the files that already existed, then we could have pointed Previous to toc in the yml above to use the same toc.yml file.

## Issues with Permalinks

Please do not use permalinks in your archive documentation Front Matter if you wish to use versioning as this will break the versioning system. If you have permalinks in your current documentation you will need to remove them when you place the documents into a subdirectory so that Jekyll can serve them correctly. 

{% include alert.html type="warning" title="Using permalinks in your archive documentation Front Matter will cause issues with versioning as this effectively 'hardcodes' the url of that page and will break the versioning links. Please be aware!" %}

## 404 Errors

There will be times where users will navigate around the documentation and could end up pointing to an invalid page that doesn't exist. For instance while you are reading this page on the **Current** version if you were to change this to the Previous version, via the dropdown, then you will get a page not found as this versioning page did not exist in the prior version. We have created a 404.md page in the base of the site so that the user still gets to navigate even though the page was not found. You can update this page with your own information should you so wish.

{% include alert.html type="info" title="When pages are not found and the 404 page is displayed the TOC menus default back to links pointing to the current release." %}