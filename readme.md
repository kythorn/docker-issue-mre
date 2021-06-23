This repository exists to provide a minimum reproducible example I ran into trying to use Docker on Win10 21H1 (19043.1052) with docker 20.10.7 (f0df350). 

Overlaying a directory that exists inside the image with a mount from the host will prevent the container from even starting with a vague error message such as:

ERROR: for demo-container  Cannot start service demo-container: container 0cb0fbde349172b597a49dd576410dc00afcd865deb02494f8445915efeb19e8 encountered an error during hcsshim::System::Start: failure in a Windows system call: The virtual machine or container exited unexpectedly. (0xc0370106)
ERROR: Encountered errors while bringing up the project.

On Win10 20H2 using a base image of mcr.microsoft.com/dotnet/framework/sdk:4.8 works as expected
On Win10 21H1 using a base image of mcr.microsoft.com/dotnet/framework/sdk:4.8 fails as outlined above
On Win10 21H2 using a base image of mcr.microsoft.com/dotnet/framework/sdk:4.8-windowsservercore-20H2 works as expected

To see the problem, using a Win10 21H1 base system, clone this repo, do a docker-compose build, and then a docker-compose up. It will fail.

To make it work, edit the Docker file and change the FROM line.

I don't know the intended difference between sdk:4.8 and sdk:4.8-windowsservercore-20H2. Browsing dockerhub I thought both tags should resolve to the some version, but they're definitely behaving differently in practice on mcr (7a7deb2dae04 vs 8eb1bd7467c3).

I've solved my immediate personal issue. Given that it fails to even start the container and fails with no meaningful error message, I am not sure where to go from here. Is this a Docker issue? Is it a Microsoft base image issue? Should a base image be able cause something like this to happen under any circumstance? This would make sense using process isolation, but I'm legitimately surprised it's happening under hyper-v.

