# BlazorFileUpload
This is a wrapper of Steve Sanderson's BlazorFileInput

This project has been updated to .NET 7.

Update 5.11.2023<br>
This project now implements DataJuggler.Blazor.Components.Interfaces, so you can register and talk to the component from a parent page or component.
To see a working project example, clone this project

BlazorGallery
https://github.com/DataJuggler/BlazorGallery

Or install via Nuget
dotnet new --install DataJuggler.BlazorGallery

Then to create a new instance
dotnet new install DataJuggler.BlazorGallery

Here is a video:

How To Create A Blazor SQL Server Image Gallery In 5 Minutes
https://youtu.be/yQz1dqYiy2g

To see another complete working example, with source code please visit:

<img src=https://excelerate.datajuggler.com/Images/ExcelerateLogoSmallWhite.png height=128 width=128>
<img src=https://excelerate.datajuggler.com/Images/logotextsparkled.png>

Blazor Excelerate <br />
https://excelerate.datajuggler.com <br />
Code Generate C# Classes From Excel Header Rows

The source code for the above project is available at:

https://github.com/DataJuggler/Blazor.Excelerate

Here is an example of creating a file upload component:

    @using DataJuggler.Blazor.FileUpload

    <FileUpload CustomSuccessMessage="Your file uploaded successfully." 
        OnReset="OnReset" ResetButtonClassName="localbutton" ShowStatus="false"
        PartialGuidLength="12" MaxFileSize=@UploadLimit FilterByExtension="true" 
        ShowCustomButton="true"  ButtonText="Upload Excel" OnChange="OnFileUploaded"
        CustomButtonClassName="@OrangeButton" AllowedExtensions=".xlsx"
        ShowResetButton="false" AppendPartialGuid="true"
        CustomExtensionMessage="Only .xlsx extensions are allowed." 
        InputFileClassName="customfileupload" Visible=false Status="Refresh"
        FileTooLargeMessage=@FileTooLargeMessage>
    </FileUpload>
    

To handle the File Upload event 'OnFileUploaded'. The code shown also starts a progress bar timer and reads the sheet names
using Nuget package DataJuggler.Excelerate (the Nuget package that powers Blazor Excelerate). 

    #region OnFileUploaded(UploadedFileInfo file)
    /// <summary>
    /// This method On File Uploaded
    /// </summary>
    public void OnFileUploaded(UploadedFileInfo file)
    {
        // if the file was uploaded
        if (!file.Aborted)
        {
            // Show the Progressbar
            ShowProgress = true;
            
            // if the ProgressBar
            if (HasProgressBar)
            {
                // Start the Timer
                ProgressBar.Start();
            }
            
            // Create a model
            GetSheetNamesModel model = new GetSheetNamesModel();
            
            // Set the model
            model.FullPath = file.FullPath;
            
            // Store this for later
            ExcelPath = file.FullPath;
            
            // reload the model
            HandleDiscoverSheets(model);
        }
        else
        {
            // for debugging only
            if (file.HasException)
            {
                // for debugging only
                string message = file.Exception.Message;
            }
        }
    }
    #endregion
    
It is on my to do list to handle multiple file uploads, I just haven't had a use case that I need this feature yet.

Volunteers are welcome to add this and I will merge the pull request. 
