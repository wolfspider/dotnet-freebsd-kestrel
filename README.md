# dotnet-freebsd-kestrel
Kestrel running on FreeBSD with FileSystemWatcher Hack

This is a workaround a developed in lieu of no [FileSystemWatcher](https://docs.microsoft.com/en-us/dotnet/api/system.io.filesystemwatcher?view=netcore-2.2) class to actually start a web server up in FreeBSD. It is based on the tech blog authored by Sergei Dorogin found [here](https://techblog.dorogin.com/aspnetcore20-serving-dot-files-27ec4730cc71) and additionally answers the question of finding hidden files in a Linux or FreeBSD environment.

The main code to look at which instantiates Watcher works like so:

    public IChangeToken Watch(string filter)
    {
        return _prodider.Watch(filter);
    }
    
I have simply prevented the Watch process from kicking off by using a null object like this:

    public IChangeToken ichnge;

    public IChangeToken Watch(string filter)
    {
        return ichnge;
    }
    
I recommend that this should be the entrance for your custom stub without having to rebuild parts of the whole ecosystem for .Net Core or heh, at least that's what I would do. With this you have the added benefit of being able to run Kestrel on FreeBSD as well.

You can see that the .csproj file is running netcoreapp30 and this is because I am using a bit of a frakenstein release I threw together myself- the instructions can be found on my post [How to build .Net Core on FreeBSD by moving files around](https://dev.to/wolfspidercode/how-to-build-net-core-on-freebsd-by-moving-files-around-53do)

It will need the 2.2 SDK and by using Preview 3 NuGet has been able to download the required dependencies. 

