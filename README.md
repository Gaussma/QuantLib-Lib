

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

File->add->New project  (Click a test project first ! )

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


TO fix why no test is showned !  
Tricks 1: 
 After Build success, if the tests has not shown in Text Explore, restart the Visual Studio 2017 ! This is the trick ! 
 Tricks 2: remove the testsuite18.cpp and pch.cpp file from the project menu !  Very important !  
(make sure an required step done ! )
 Trick 3:  
 Configuration Properties -> General -> 
 Output Directory :  \bin  
 after this set, the text explore can find it ! !! 



$(SolutionDir)$(Platform)\$(Configuration)\-->\bin 


//on the remote side ! 

 Unknown compiler version - please run the configure tests and report the results
1753
  timegrid.cpp
1754
  Unknown compiler version - please run the configure tests and report the results
1755
  Make build directory
1756
  QuantLib.vcxproj -> C:\projects\quantlib-lib\.\lib\QuantLib-vc141-x64-mt.lib
1757
Discovering tests...OK
1758
Build success






Jan 8 , 2019 

1. 
 Set the enviroment variable $(BOOST_ROOT), $(BOOST_LIBRARYDIR), $(QuantLib1) to user Enviroment, not system variable in local. 

2. replace the absolute path with Enviroment variable $(QuantLib1) in testsuite12. ()

After replacement, utlities.hpp can not find ql/ directories. 
After we restart the VS 2017 . Built success !  

Track: After set the enviroment variable and use enviroment variable in the VS , we need to restart the Visual Studio !  



1. The output of QuatnLib Library is .\lib\



For StaticLibrary :  
We update the Configuration Properties->General->OutputDirectory 

$(SolutionDir)$(Platform)\$(Configuration)\  to \bin 





QuantLib.vcxproj -> C:\projects\quantlib-lib\.\lib\QuantLib-vc141-x64-mt.lib



***********

Windows cmd prompt: (You could try the below command directly in windows cmd if you are not comfortable with grep, rm -rf, find, xargs etc., commands in git bash )
Delete .git recursively inside the project folder by the following command in cmd: 
FOR /F "tokens=*" %G IN ('DIR /B /AD /S .git') DO RMDIR /S /Q "%G"


***********



Now I create a new solution:


VC++ Directory  Inclucde Directories : $(MathLib) 
                Reference Directories : $(MathLib)/lib

C++ : BOOST_ROOT  




Multi-threaded DLL (/MD)




Build a C++ library , linking it, then 
another program call this library.  



utilities.hpp contains 


#include <ql/instruments/payoffs.hpp>
   #include <ql/option.hpp>
      #include <ql/instrument.hpp>
         (#include <ql/patterns/lazyobject.hpp>
         	[#include <ql/patterns/observable.hpp>]
         	 {#include <ql/errors.hpp>
         	 	  (#include <ql/qldefines.hpp>-none 
                   #include <ql/shared_ptr.hpp>
                       [<ql/qldefines.hpp>]-meet)
               #include <ql/types.hpp>
                  (#include <ql/qldefines.hpp> meet
               	  )
               #include <ql/patterns/singleton.hpp>
                  (#include <ql/qldefines.hpp>-meet)

               #include <ql/shared_ptr.hpp> -meet}
          #include <ql/pricingengine.hpp>
            [#include <ql/patterns/observable.hpp>]-meet
          #include <ql/utilities/null.hpp>
            [#include <ql/types.hpp>]-meet 
          #include <ql/time/date.hpp>)
            [#include <ql/time/period.hpp>
                  {#include <ql/time/frequency.hpp>
                  	(#include <ql/qldefines.hpp>)-meet
                   #include <ql/time/timeunit.hpp>
                    (#include <ql/qldefines.hpp>)-meet
                   #include <ql/types.hpp>-meet}
             #include <ql/time/weekday.hpp>
                 (#include <ql/qldefines.hpp>-meet)
              #include <ql/utilities/null.hpp>-meet]
   #include <ql/payoff.hpp>
      #include <ql/types.hpp>
         (#include <ql/qldefines.hpp>)-meet
      #include <ql/patterns/visitor.hpp>
         (#include <ql/qldefines.hpp>)-meet
      #include <ql/errors.hpp>
          (#include <ql/qldefines.hpp> - meet
           #include <ql/shared_ptr.hpp>)-meet 
#include <ql/exercise.hpp>
      {#include <ql/time/date.hpp> -meet}
#include <ql/termstructures/yieldtermstructure.hpp>
      { #include <ql/termstructure.hpp>-solved
      	    [#include <ql/time/calendar.hpp>-solved. 
      	       (#include <ql/errors.hpp>-meet 
                  #include <ql/time/date.hpp>-meet 
                 #include <ql/time/businessdayconvention.hpp>
                  {#include <ql/qldefines.hpp> -meet }
                  #include <ql/shared_ptr.hpp>-meet )
             #include <ql/time/daycounter.hpp> solved/
                (#include <ql/time/date.hpp> meet 
                 #include <ql/errors.hpp> meet )
            #include <ql/settings.hpp> solved
                (#include <ql/patterns/singleton.hpp> meet
                #include <ql/time/date.hpp> meet 
               #include <ql/utilities/observablevalue.hpp>
                {#include <ql/patterns/observable.hpp> meet})
            #include <ql/handle.hpp> solved
                (<ql/patterns/observable.hpp>-meet)
            #include <ql/math/interpolations/extrapolation.hpp>
             (<ql/qldefines.hpp> meet)
             #include <ql/utilities/null.hpp> meet]
        #include <ql/interestrate.hpp> solved
            [#include <ql/compounding.hpp>
              (#include <ql/qldefines.hpp> meet)
             #include <ql/time/daycounters/actual365fixed.hpp>
             (#include <ql/time/daycounter.hpp> meet)
             ]
        #include <ql/quote.hpp> solved
           [#include <ql/handle.hpp> meet
            #include <ql/errors.hpp> meet 
            #include <ql/utilities/null.hpp> meet]}
    #include <ql/termstructures/volatility/equityfx/blackvoltermstructure.hpp> solved
     {#include <ql/termstructures/voltermstructure.hpp>
     	[#include <ql/termstructure.hpp> meet ]
      #include <ql/patterns/visitor.hpp>solved. 
       [#include <ql/qldefines.hpp> meet]}
#include <ql/quote.hpp>-meet 
#include <ql/patterns/observable.hpp> -meet 
#include <ql/time/daycounters/actual365fixed.hpp>-meet 






