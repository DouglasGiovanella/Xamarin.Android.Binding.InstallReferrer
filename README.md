# Xamarin.Android.Binding.InstallReferrer

A Google's InstallReferrer library binding for Xamarin Android

Official docs: https://developer.android.com/google/play/installreferrer/library

Example of use:

````c#
InstallReferrerClient referrerClient = InstallReferrerClient.NewBuilder(context).Build();

referrerClient.StartConnection(new InstallReferrerStateListenerImpl {
    InstallReferrerSetupFinished = code => OnSetupFinished(referrerClient, code),
    InstallReferrerServiceDisconnected = OnInstallReferrerServiceDisconnected
});

private static void OnSetupFinished(InstallReferrerClient referrerClient, int code) {
    switch (code) {
        case InstallReferrerClient.InstallReferrerResponse.DeveloperError:
            TrackError("DeveloperError");
            break;
        case InstallReferrerClient.InstallReferrerResponse.PermissionError:
            TrackError("PermissionError");
            break;
        case InstallReferrerClient.InstallReferrerResponse.ServiceDisconnected:
            TrackError("ServiceDisconnected");
            break;
        case InstallReferrerClient.InstallReferrerResponse.ServiceUnavailable:
            TrackError("ServiceUnavailable");
            break;
        case InstallReferrerClient.InstallReferrerResponse.FeatureNotSupported:
            TrackError("FeatureNotSupported");
            break;
        case InstallReferrerClient.InstallReferrerResponse.Ok:
            OnConnectionEstablished(referrerClient);
            break;
    }
}

private static void OnConnectionEstablished(InstallReferrerClient referrerClient) {
    ReferrerDetails response = referrerClient.InstallReferrer;
    string referrerUrl = response.InstallReferrer;
    
    // do something
    
    referrerClient?.EndConnection();
    referrerClient?.Dispose();
}
````

````c#
 internal sealed class InstallReferrerStateListenerImpl : Java.Lang.Object, IInstallReferrerStateListener {

    public Action<int> InstallReferrerSetupFinished;
    public Action InstallReferrerServiceDisconnected;

    public void OnInstallReferrerServiceDisconnected() {
        InstallReferrerServiceDisconnected.Invoke();
    }

    public void OnInstallReferrerSetupFinished(int p0) {
        InstallReferrerSetupFinished.Invoke(p0);
    }

}
````

