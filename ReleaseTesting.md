# Introduction #

Before a release is published, the distributed package should be tested against all supported platforms and Python versions. When all issues scheduled for the release-related milestone are closed, the release is frozen. The source archive is deployed on the target platforms and pre- and post-installation tests are run. The test results are recorded in the Test Matrix below. If some tests fail, the Test Matrix is cleared once all issues are resolved and all tests are rerun.

## Release Branching ##

The release branching strategy loosely follows the Subversion [Release Branches](http://svnbook.red-bean.com/en/1.5/svn-book.html#svn.branchmerge.commonpatterns.release) guideline.

Once all release-critical issues are closed, a svn branch is created to prepare the new release:

```
svn copy https://pympler.googlecode.com/svn/trunk https://pympler.googlecode.com/svn/branches/0.1 -m "Creating release branch."
```

The branch allows to keep on working on the trunk while the release is being tested. To ensure that all tests are run against the same revision a release-candidate tag is created from the branch:

```
svn copy https://pympler.googlecode.com/svn/branches/0.1 https://pympler.googlecode.com/svn/tags/0.1rc1 -m "Creating release-candidate."
```

If problems/bugs are found during release-testing, it is fixed in the trunk and then selectively merged into the release branch. Once all problems were fixed, another release candidate is created and tested.

If all tests pass, the release candidate is converted to the actual release.

```
svn move https://pympler.googlecode.com/svn/tags/0.1rc1 https://pympler.googlecode.com/svn/tags/0.1 -m "Relabel release-candidate as release."
```

## How to run the test ##

  1. checkout the release-candidate tag
  1. run setup.py with pre- and post-installation testing
    * Test fails
      * Issue should be opened in the bug tracker
      * Issue number is recorded in the test matrix
    * All tests pass
      * Write 'PASSED' to the associated cell in the test matrix

```
svn checkout http://pympler.googlecode.com/svn/tags/0.2.0rc4 pympler0.2
cd pympler0.2
python run.py --dist --html --keep
```

If you have Bash, you can also run the whole test suite for every installed Python version from within the working copy root folder:

```
bash tools/release_testing.sh
```

Deploy the dist archive at the test systems, unpack, run tests, and install for each Python version separately.

```
tar xzf Pympler-0.1.tar.gz
cd Pympler-0.1
python2.4 setup.py try
python2.4 setup.py install
python2.4 setup.py test
rm -rf *
cd ..
```

# Test Matrix #

## pympler 0.2.0rc4 ##

| | **Python 2.4** | **Python 2.5** | **Python 2.6** | **Python 2.7** | **Python3.1** | **Python 3.2** |
|:|:---------------|:---------------|:---------------|:---------------|:--------------|:---------------|
| Ubuntu 10.04 32bit | PASSED         | PASSED         | PASSED         | PASSED         | PASSED        | PASSED         |
| Ubuntu/Debian 64bit |                |                |                |                |               |                |
| Windows XP 32bit |                |                |                |                |               |                |
| Windows 7 32bit |                | PASSED         |                | PASSED         |               |                |
| Windows 7 64bit |                |                | PASSED         | PASSED         | PASSED        | PASSED         |
| MacOS X 10.6 32bit |                |                |                |                |               |                |
| MacOS X 10.6 64bit |                |                |                |                |               |                |
| Solaris 10 |                |                |                |                |               |                |

## pympler 0.2.0rc3 ##

| | **Python 2.4** | **Python 2.5** | **Python 2.6** | **Python 2.7** | **Python3.1** | **Python 3.2** |
|:|:---------------|:---------------|:---------------|:---------------|:--------------|:---------------|
| Ubuntu 10.04 32bit | PASSED         | PASSED         | PASSED         | PASSED         | PASSED        | PASSED         |
| Windows 7 32bit |                | [issue 42](https://code.google.com/p/pympler/issues/detail?id=42) <sup>[AP]</sup> |                | [issue 42](https://code.google.com/p/pympler/issues/detail?id=42) <sup>[AP]</sup> |               |                |
| Windows 7 64bit |                |                | PASSED         | [issue 42](https://code.google.com/p/pympler/issues/detail?id=42) <sup>[AP]</sup> | PASSED        | [issue 42](https://code.google.com/p/pympler/issues/detail?id=42) <sup>[AP]</sup> |
| MacOS X 10.6 32bit |                | PASSED         |                |                |               |                |
| MacOS X 10.6 64bit |                |                | [issue 41](https://code.google.com/p/pympler/issues/detail?id=41) |                |               |                |

`[AP]` ActivePython including pywin32 extension

## pympler 0.2.0rc2 ##

| | **Python 2.4** | **Python 2.5** | **Python 2.6** | **Python 2.7** | **Python3.1** | **Python 3.2** |
|:|:---------------|:---------------|:---------------|:---------------|:--------------|:---------------|
| Ubuntu 10.04 32bit | [issue 39](https://code.google.com/p/pympler/issues/detail?id=39) | PASSED         | PASSED         | PASSED         | PASSED        | [issue 40](https://code.google.com/p/pympler/issues/detail?id=40) |
| Windows 7 64bit |                |                | PASSED         |                | PASSED        |                |

## pympler 0.2.0rc1 ##

| | **Python 2.4** | **Python 2.5** | **Python 2.6** | **Python 2.7** | **Python3.1** | **Python 3.2** |
|:|:---------------|:---------------|:---------------|:---------------|:--------------|:---------------|
| Windows 7 64bit |                |                | [issue 38](https://code.google.com/p/pympler/issues/detail?id=38) |                |               |                |

## pympler 0.1 ##

| | **Python 2.4** | **Python 2.5** | **Python 2.6** |
|:|:---------------|:---------------|:---------------|
| Ubuntu 8.10 32bit | PASSED         | PASSED         | PASSED         |
| Ubuntu 8.10 64bit | PASSED         | PASSED         | PASSED         |
| Debian 64bit | PASSED         | PASSED         | PASSED         |
| MacOS X  10.4.11 (Intel) 32bit |  PASSED        | PASSED         | PASSED         |
| Windows XP 32bit | PASSED|  PASSED        | PASSED         |
| Solaris  10 (x86 Opteron) 32bit |  PASSED        | PASSED         | PASSED         |