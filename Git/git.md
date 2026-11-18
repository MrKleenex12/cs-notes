# Git - the stupid content tracker

Git is a version control system (VCS). What is a VCS? It is a system that
tracks versions of a project over time. The project can be anything from code,
image files, text files, etc. Any file type you need to track versions of is a
reasonable use for git.

Git didn't just appear out of thin air. It had a creation story like most
things in life. Long story short, git is a distributed version control system
(a specific flavor) that had these goals in mind:

- Speed
- Strong support for parallel development
- Simple design
- Fully distributed

Read more about the history of Git:

- [A Short History of Git](https://git-scm.com/book/en/v2/Getting-Started-A-Short-History-of-Git)
- [Git Wikipedia](https://en.wikipedia.org/wiki/Git)

## How does Git work

The biggest difference between Git and another VCS is how Git stores changes.
Most systems store data as a list of file-based changes. This is called
*delta-based* version control.

Git stores its data as a series of snapshots. Think of a snapshot as a backup
of your current directory in each version. To save storage, files with no changes
just store a link to the previous version. In all things considered, this makes
git act more like a mini filesystem instead of just a VCS.

## Local operations

Git is blazing fast because it has no network overhead. Git doesn't need to
access the network in order to complete most operations. Every bit of history
is on your local disk.

This means most operations are uneffected by your network connection. Need to
find difference in changes from now to a month ago? Pull both up versions from
your local disk. Need track any current changes? Save it to your *local*
database (to be uploaded later). This reduces the number of bottlenecks in your
workflow.

## Checksum Integrity

One of Git's strengths is it's integrity. No changes to a file or directory's
content should be made without Git knowing about it. This ensures that a repo's
content stays consistent through accidental damages and deliberate tampering
from someone.

Git uses a SHA-1, a cryptographic hash function. It takes the input and produces
a 160-bit (20-byte) hash value. It is a 40-character string composed of
hexidecimal characters (0-9 or A-E).

SHA-1 has been deemed unsafe, but is still the default option for git. You can
still expect that git tracks every change correctly.

Read here for more details on SHA-1 collision:

- [The SHAttered SHA-1 Collision, Explained](https://shattered.io/sha1-collision/)
- [Announcing the first SHA1 collision](https://security.googleblog.com/2017/02/announcing-first-sha1-collision.html)

## Three States

In Git, your files have three possible states: *modified*, *staged*, and *committed*

- Modified: made updates to file, but not committed to database yet
- Staged: marked a modified file in its currrent version to next commit snapshot
- Committed: data is safely stored in local database

![Working tree, staging area, and Git directory](https://git-scm.com/book/en/v2/images/areas.png)

*Figure: [The three Git states](https://git-scm.com/book/en/v2/images/areas.png),
from Scott Chacon and Ben Straub, [*Pro Git*](https://git-scm.com/book/en/v2).*

The working tree is a single checkout of one version of the project. These
files are pulled out of the compressed database in the Git directory and
placed on disk for you to use or modify.

The staging area is a file that stores what will go into the next commit.
It's actual name is the "index".

The Git directory (.git) is where the metadata and object database. It gets
copied when you clone a repository from another computer. .git contains:

5 sub-directories:

- hooks/ - example scripts
- info/ - `exclude` for ignored patterns
- objects/ - objects that link together to blobs, trees, and commits
- refs/ - pointers for branches (heads), remotes, and tags
- logs/ - stores reflog: a local history of where branch and `HEAD` references
move. Also keeps track of last known commit for remotes

config - text file of repo specific Git settings

index - Git binary for its staging area. It records what changes are prepared
for the next commit
