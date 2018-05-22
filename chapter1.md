\[toc\]

\# Use

\#\# Create

Inside a markdown file, the first time, place yourself where you want to insert the TOC:

&lt;kbd&gt;M-x markdown-toc-generate-toc&lt;\/kbd&gt;

This will compute the TOC and insert it at current position.

You can also execute: &lt;kbd&gt;M-x markdown-toc-generate-or-refresh-toc&lt;\/kbd&gt; to either

gnerate a TOC when none exists or refresh the currently existing one.

Here is one possible output:

\`\`\`markdown

&lt;!-- markdown-toc start - Don't edit this section. Run M-x markdown-toc-refresh-toc --&gt;

\*\*Table of Contents\*\*

* \[Use\]\(\#use\)

  * \[Create\]\(\#create\)

  * \[Update\]\(\#update\)

  * \[Create elsewhere\]\(\#create-elsewhere\)



* \[Install\]\(\#install\)

  * \[emacs package repository\]\(\#emacs-package-repository\)

  * \[Setup\]\(\#setup\)

  * \[melpa stable\]\(\#melpa-stable\)

  * \[melpa\]\(\#melpa\)

  * \[marmalade\]\(\#marmalade\)

  * \[Install\]\(\#install\)

  * \[emacs-lisp file\]\(\#emacs-lisp-file\)



* \[Inspiration\]\(\#inspiration\)

\`\`\`

\#\# User toc manipulation

If the user would want to enhance the generated toc, \(s\)he could use

the following function markdown-toc-user-toc-structure-manipulation-fn:

It expects as argument the toc-structure markdown-toc uses to generate

the toc. The remaining code expects a similar structure.

Example:

\`\`\` lisp

'\(\(0 . "some markdown page title"\)

\(0 . "main title"\)

\(1 . "Sources"\)

\(2 . "Marmalade \(recommended\)"\)

\(2 . "Melpa-stable"\)

\(2 . "Melpa \(~snapshot\)"\)

\(1 . "Install"\)

\(2 . "Load org-trello"\)

\(2 . "Alternative"\)

\(3 . "Git"\)

\(3 . "Tar"\)

\(0 . "another title"\)

\(1 . "with"\)

\(1 . "some"\)

\(1 . "heading"\)\)

\`\`\`

So for example, as asked in \#16, one could drop the first element:

\`\`\` lisp

\(custom-set-variables '\(markdown-toc-user-toc-structure-manipulation-fn 'cdr\)\)

\`\`\`

Or drop all h1 titles... or whatever:

\`\`\` lisp

\(require 'dash\)

\(custom-set-variables '\(markdown-toc-user-toc-structure-manipulation-fn

\(lambda \(toc-structure\)

\(-filter \(lambda \(l\) \(let \(\(index \(car l\)\)\)

\(&lt;= 1 index\)\)\)

toc-structure\)\)\)

\`\`\`

\#\# Update

To update the existing TOC, simply execute:

&lt;kbd&gt;M-x markdown-toc-refresh-toc&lt;\/kbd&gt;

This will update the current TOC.

\#\# Create elsewhere

To create another updated TOC elsewhere, execute &lt;kbd&gt;M-x

markdown-toc-generate-toc&lt;\/kbd&gt; again, this will remove the old TOC and

insert the updated one from where you stand.

\#\# Remove

To remove a TOC, execute &lt;kbd&gt;M-x markdown-toc-delete-toc&lt;\/kbd&gt;.

\#\# Customize

Currently, you can customize three variables:

\* \`markdown-toc-header-toc-start\`

\* \`markdown-toc-header-toc-title\`

\* \`markdown-toc-header-toc-end\`

Customize them as following format:

\`\`\` lisp

\(custom-set-variables

'\(markdown-toc-header-toc-start "&lt;!-- customized start--&gt;"\)

'\(markdown-toc-header-toc-title "\*\*customized title\*\*"\)

'\(markdown-toc-header-toc-end "&lt;!-- customized end --&gt;"\)\)

\`\`\`

\# Install

\#\# emacs package repository

You need to add melpa or melpa-stable package repository before installing it.

\#\#\# Setup

\#\#\#\# melpa stable

\`\`\` lisp

\(require 'package\)

\(add-to-list 'package-archives '\("melpa-stable" .

"http:\/\/melpa-stable.milkbox.net\/packages\/"\)\)

\(package-initialize\)

\`\`\`

Then hit &lt;kbd&gt;M-x eval-buffer&lt;\/kbd&gt; to evaluate the buffer's contents.

\#\#\#\# melpa

\`\`\` lisp

\(require 'package\)

\(add-to-list 'package-archives '\("melpa" .

"http:\/\/melpa.milkbox.net\/packages\/"\)\)

\(package-initialize\)

\`\`\`

Then hit &lt;kbd&gt;M-x eval-buffer&lt;\/kbd&gt; to evaluate the buffer's contents.

\#\#\#\# marmalade

\`\`\` lisp

\(require 'package\)

\(add-to-list 'package-archives '\("marmalade" .

"http:\/\/marmalade-repo.org\/packages\/"\)\)

\(package-initialize\)

\`\`\`

\#\#\# Install

&lt;kbd&gt;M-x package-install RET markdown-toc RET&lt;\/kbd&gt;

\#\# emacs-lisp file

Retrieve the markdown-toc.el https:\/\/github.com\/ardumont\/markdown-toc\/releases.

Then hit &lt;kbd&gt;M-x package-install-file RET markdown-toc.el RET&lt;\/kbd&gt;

\# Inspiration

https:\/\/github.com\/thlorenz\/doctoc

The problem I had with doctoc is the installation process.

I do not want to install the node tools just for this.

