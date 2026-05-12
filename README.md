# Visual Studio Static HTML Bug

This repository is meant to explain and show how to reproduce a bug in Visual Studio that is preventing static HTML files from being served correctly in web projects.

## Summary of the Issue

When using `app.MapStaticAssets()` in a .NET web application (API, Blazor, Blazor WASM), navigating to a static HTML file while running in Visual Studio causes an exception to be thrown. Running the same project using the `dotnet` CLI does not result in any issues.

The exception: System.IO.InvalidDataException: The archive entry was compressed using an unsupported compression method.

## Reproduction

### Setup

*These are the steps I took to create this repository. You can skip these steps if you clonse this repository.*

1. Create a folder to hold this project and open the terminal to that folder
2. Create the solution using this command:
    - `dotnet new sln --name VisualStudioStaticHtmlBug`
3. Create the project using this command:
    - `dotnet new webapi --name VisualStudioStaticHtmlBug`
4. Add the project to the solution using this command:
    - `dotnet sln add VisualStudioStaticHtmlBug`
5. Create a folder named `wwwroot` in the project
6. Add an HTML file with some HTML content to the `wwwroot` folder (you can copy the `hello-world.html` file from this repository)
7. Add `app.MapStaticAssets();` to `Program.cs` on the line after `app.UseHttpsRedirection();`

### Testing

1. Open and run the project in Visual Studio.
2. Navigate to the static HTML file in a browser.
3. The browser will show a 500 error and the application will show an exception.
    - Expected exception: System.IO.InvalidDataException: The archive entry was compressed using an unsupported compression method.

## Things to Notice

- The error only happens if accessing the HTML file using a browser. `curl` does not trigger the exception.
- Running the project with the `dotnet` CLI does not result in any issues, even when both use the same launch profile, web server (Kestrel), and SDK version.
- This issue affects more than just Web API projects. It also affects Blazor and Blazor WASM (and possibly more project types).
- When using `app.UseStaticFiles();` instead of `app.MapStaticAssets();`, the issue does not occur.

## My Version Info

- OS: Windows 11 Pro 25H2
- .NET Version: 10.0.204
- Visual Studio 2026 Version: 18.5.3
