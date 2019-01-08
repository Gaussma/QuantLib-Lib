

QuantLib: the free/open-source library for quantitative finance
===============================================================

[![Download](https://api.bintray.com/packages/quantlib/releases/QuantLib/images/download.svg)](https://bintray.com/quantlib/releases/QuantLib/_latestVersion)
[![Build Status](https://travis-ci.org/lballabio/QuantLib.svg?branch=master)](https://travis-ci.org/lballabio/QuantLib)
[![Codacy Badge](https://api.codacy.com/project/badge/Grade/b4bc1058db994f24aa931b119a885eea)](https://www.codacy.com/app/lballabio/QuantLib)
[![codecov](https://codecov.io/gh/lballabio/QuantLib/branch/master/graph/badge.svg)](https://codecov.io/gh/lballabio/QuantLib)
[![Average time to resolve an issue](http://isitmaintained.com/badge/resolution/lballabio/QuantLib.svg)](http://isitmaintained.com/project/lballabio/QuantLib)

---


Jan 7 2019  

The QuantLib project (<http://quantlib.org>) is aimed at providing a
comprehensive software framework for quantitative finance. QuantLib is
a free/open-source library for modeling, trading, and risk management
in real-life.

QuantLib is Non-Copylefted Free Software and OSI Certified Open Source
Software.


Download and usage
------------------

QuantLib can be downloaded from <http://quantlib.org/download.shtml>;
installation instructions are available at
<http://quantlib.org/install.shtml> for most platforms.

Documentation for the usage and the design of the QuantLib library is
available from <http://quantlib.org/docs.shtml>.

A list of changes for each past versions of the library can be
browsed at <http://quantlib.org/reference/history.html>.


Questions and feedback
----------------------

The preferred channel for questions (and the one with the largest
audience) is the quantlib-users mailing list.  Instructions for
subscribing are at <http://quantlib.org/mailinglists.shtml>.

Bugs can be reported as a GitHub issue at
<https://github.com/lballabio/QuantLib/issues>; if you have a patch
available, you can open a pull request instead (see "Contributing"
below).


Contributing
------------

The preferred way to contribute is through pull requests on GitHub.
Get a GitHub account if you don't have it already and clone the
repository at <https://github.com/lballabio/QuantLib> with the "Fork"
button in the top right corner of the page. Check out your clone to
your machine, code away, push your changes to your clone and submit a
pull request; instructions are available at
<https://help.github.com/articles/fork-a-repo>.

In case you need them, more detailed instructions for creating pull
requests are at
<https://help.github.com/articles/using-pull-requests>, and a basic
guide to GitHub is at
<https://guides.github.com/activities/hello-world/>.  GitHub also
provides interactive learning at <https://lab.github.com/>.

It's likely that we won't merge your code right away, and we'll ask
for some changes instead. Don't be discouraged! That's normal; the
library is complex, and thus it might take some time to become
familiar with it and to use it in an idiomatic way.

We're looking forward to your contributions.


Jan 9  .

1. Today I download the QuantLib again.  

Built the QuantLib library first. 

Right Click the Project "QuantLib" -> properties 

1) 
VC++ ->  Include Directories -> add $(BOOST_ROOT)
(NO NEED BOOST_LIBRARYDIR in this step)

2) C/C++ ->General-> Additional Include Directories -> add $(BOOST_ROOT) . Rememebr there is a "." in here, do not move ! since it can call all the ql/ directories and files for quantlib itself. Othervise, it has compile error. 

3) we do not need to set a linker to built QuantLib Library !
The linker is only for test case !  

*********************************************

Now We built the QuantLib Library part in Appveryor 
To make sure the system are the same between local computer and remote . 
on Local computer: Release , x64 platform   
In Appveyor : 
1) Enviroment Settings, set 
 $(BOOST_ROOT) :  C:\Libraries\boost_1_64_0
 $(BOOST_LIBRARYDIR): C:\Libraries\boost_1_64_0\lib64-msvc-14.1    

 Please make sure correct 59 to 64 and lib64-msvc-14.0 to lib64-msvc-14.1 .   Othervise the remote can not find the boost library .  

2) Build :  
 Visual Studio 2017 
 Release
 x64 

***********************

Now I built the QuantLib library itself in my home computer, to generate the exectuable .lib in the lib folder 



FINISH! 

For test case ! 

DO NOT FORGET  TO ADD THE QUANTLIB AS REFERENCE !  


Currently, the test can not shown !  

***************************

Step by step instruction to create testsuite18  
1. create the project . 
Click File , Choose Visual C++, Windows Console Application. 
Name : testsuite18 

Location : The same location of the solution. In here 
we choose C:\Users\Xiaoyao\Documents\ThirdParty\QuantLib-1.14\  

Solution : Addition to Solution

Solution name is default since we do not add a new solution. 

SO in this way, the testsuite18 is listed parallel to testsuite.  

Now we copy the following files from testsuite to testsuite18 : 

2. copy the files . 
calendar.hpp
speedlevel.hpp 
utilities.hpp

calendar.cpp
calendar.cpp
utilities.cpp
quantlibtestsuite.cpp


Note: we should comment out the unused the function and hpp files in quantlibtestsuite.cpp 

3. set the project configurations. 
Then the important step is to set up the properties configurations for the project testsuite18. 


Since we have set $(BOOST_ROOT) and $(BOOST_LIBRARYDIR) in the local environment variables. 

We have 
$(BOOST_ROOT)=C:\Libraries\boost_1_68_0

$(BOOST_LIBRARYDIR)=C:\Libraries\boost_1_68_0\stage_x86\lib

Right click on Project testsuite18 , choose "Properties"
3.1 Make sure the platform of the project are match with the solution Quantlib. Otherwise it make zero sense !  This is avery important point. 

3.2 Configuration 
VC++ Directories:
 Include Directories : 
 $(BOOST_ROOT);
 C:\Users\Xiaoyao\Documents\ThirdParty\QuantLib-1.14;$(IncludePath)

 Library Directories: 
 C:\Users\Xiaoyao\Documents\ThirdParty\QuantLib-1.14\lib;$(LibraryPath)

 $(BOOST_ROOT)\libs is optional, it can built success without it ! 

 C/C++ : Additional Include Directories: 
 $(BOOST_ROOT);
 C:\Users\Xiaoyao\Documents\ThirdParty\QuantLib-1.14;%(AdditionalIncludeDirectories)

C/C++ : Precompiled Headers : Not Using Precompiled Headers 


Linker: Additional Library Directories :
         $(BOOST_LIBRARYDIR)


 4. Build the Project testsuite18. 


 When build the testsuite18, do not click the "Build" directly !
 Right Click the project testsuite18, --> "Project Only"
-->"Build Only " !  

Tricks 1: 
 After Build success, if the tests has not shown in Text Explore, restart the Visual Studio 2017 ! This is the trick ! 
 Tricks 2: remove the testsuite18.cpp and pch.cpp file from the project menu !  
(make sure an required step done ! )
 Trick 3:  
 Configuration Properties -> General -> 
 Output Directory :  \bin  
 after this set, the text explore can find it ! !! 



$(SolutionDir)$(Platform)\$(Configuration)\















*
