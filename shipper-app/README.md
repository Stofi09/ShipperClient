# ShipperApp

This project was generated with [Angular CLI](https://github.com/angular/angular-cli) version 17.0.8.

## Development server

Run `ng serve` for a dev server. Navigate to `http://localhost:4200/`. The application will automatically reload if you change any of the source files.

## Code scaffolding

Run `ng generate component component-name` to generate a new component. You can also use `ng generate directive|pipe|service|class|guard|interface|enum|module`.

## Build

Run `ng build` to build the project. The build artifacts will be stored in the `dist/` directory.

## Running unit tests

Run `ng test` to execute the unit tests via [Karma](https://karma-runner.github.io).

## Running end-to-end tests

Run `ng e2e` to execute the end-to-end tests via a platform of your choice. To use this command, you need to first add a package that implements end-to-end testing capabilities.

## Further help

To get more help on the Angular CLI use `ng help` or go check out the [Angular CLI Overview and Command Reference](https://angular.io/cli) page.

public void ZipAndUpload(string file1Path, string file2Path, string remotePath)
{
    using (var sftp = new SftpClient(host, username, password))
    {
        try
        {
            sftp.Connect();

            string tempZipPath = Path.GetTempFileName();

            using (var zip = ZipFile.Open(tempZipPath, ZipArchiveMode.Create))
            {
                zip.CreateEntryFromFile(file1Path, Path.GetFileName(file1Path));
                zip.CreateEntryFromFile(file2Path, Path.GetFileName(file2Path));
            }

            using (var fileStream = new FileStream(tempZipPath, FileMode.Open))
            {
                sftp.UploadFile(fileStream, remotePath);
            }

            // Optional: Check if the upload was successful by comparing file sizes
            var localFileInfo = new FileInfo(tempZipPath);
            var remoteFileInfo = sftp.GetAttributes(remotePath);
            if (remoteFileInfo != null && localFileInfo.Length == remoteFileInfo.Size)
            {
                Console.WriteLine("Upload successful and file sizes match.");
            }
            else
            {
                Console.WriteLine("Upload may have failed or file sizes do not match.");
            }

            File.Delete(tempZipPath);

            sftp.Disconnect();
        }
        catch (Exception ex)
        {
            Console.WriteLine("Error: " + ex.Message);
        }
    }
}
}
@model IEnumerable<YourNamespace.YourModel>

@foreach (var item in Model) {
    <div class="clickable-div" data-item-id="@item.Id">@Html.DisplayFor(modelItem => item.Name)</div>
}

<style>
.clickable-div {
    cursor: pointer;
    /* Add more styling here */
}
</style>

<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
<script>
$(document).ready(function() {
    $('.clickable-div').click(function() {
        var itemId = $(this).data('item-id'); // Get the item ID
        $.ajax({
            url: '/YourController/YourAction',
            type: 'POST',
            contentType: 'application/json',
            data: JSON.stringify({ id: itemId }),
            success: function(response) {
                console.log(response); // Handle success
            },
            error: function(error) {
                console.error(error); // Handle error
            }
        });
    });
});
</script>


// Usage example:
// var uploader = new SftpUploader("sftp.example.com", "username", "password");
// uploader.ZipAndUpload(@"C:\path\to\file1.txt", @"C:\path\to\file2.txt", "/remote/path/destination.zip");