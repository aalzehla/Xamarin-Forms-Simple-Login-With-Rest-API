# Xamarin Forms Simple Login With Rest API

This project is one of the simplest ways how to implement login system using Web Service Rest API.

Before we begin, we need to prepare our Web Service.

I created an optional prerequisite in creating your basic web service api using ASP.NET Web API.

Here's the link: https://github.com/deanilvincent/StepsInCreatingBasicASP.NETWebAPI

In this sample web service project, I included these following model class properties:

``` html
public class UserDetailCredentials
{
    public int Id { get; set; }
    public string Username { get; set; }
    public string Password { get; set; }
    public string Name { get; set; }
}
```
When you are done creating your web service, you can now proceed in creating our Xamarin Cross Platform Mobile App project.

# Basic Steps in Implementing Login System using Xamarin Cross Platform.

### 1.0 Creating the project


1.1 Go to File -> New -> Project (Ctrl+Shift+N)

1.2 Choose Templates -> Visual C# -> Cross Platform and rename the project. (Here I named my project as CrossPlatformBasicLoginSystem) and then Click the "OK"

![capture24](https://cloud.githubusercontent.com/assets/10904957/23104382/c77b7d80-f707-11e6-8e12-cd104b43dd02.PNG)

1.3 A white dialog box will appear and you need to choose the following: Template Blank App, Forms in UI Technology and PCL in Sharing. Then click "Accept" to finalize the project creation.

![capture25](https://cloud.githubusercontent.com/assets/10904957/23104393/0c987d00-f708-11e6-85a4-6b668a59bbfa.PNG)

ow you have successfull created your project. This includes the following projects summary:

1. Main Project Portable (PCL)
2. Project.Droid (Android)
3. Project.iOS (iOS)
4. Project.UWP (UWP)

![capture27](https://cloud.githubusercontent.com/assets/10904957/23104460/4d423c00-f709-11e6-97dc-4de05a273c21.PNG)

### 2.0 Installation of nuget packages


2.1 Install the following nuget packages at the main project solution:

- Microsoft.Net.Http (https://www.nuget.org/packages/Microsoft.Net.Http/)


> Project URL: http://go.microsoft.com/fwlink/?LinkID=280055
    
- Microsoft.Bcl (https://www.nuget.org/packages/Microsoft.Bcl/)

    > Project URL: http://go.microsoft.com/fwlink/?LinkID=280057
    
5- Microsoft.Bcl.Build (https://www.nuget.org/packages/Microsoft.Bcl.Build/)

    > Project URL: http://go.microsoft.com/fwlink/?LinkID=296436


You can use either the Manage Packages for Solution or Manually Type in the packages in the Package Manager Console. 

Note: The simplest way is to just install the Microsoft.Net.Http since Microsoft.BCL & Microsoft.Bcl.Build are part of its dependencies.

![capture29](https://cloud.githubusercontent.com/assets/10904957/23104700/e8d106c0-f70d-11e6-95b3-cb33e834d34f.PNG)

2.2 Create the following folders in the Portable Project

- Models
- RestAPIClient
- ServicesHandler
- Views

![capture30](https://cloud.githubusercontent.com/assets/10904957/23104875/ef96ccbc-f710-11e6-8ab5-f9f537716ab7.PNG)

### 3.0 Creating the Model Class

3.1 Right click the Models Folder -> Add -> Class... In this example, we name our class model as "UserDetailCredentials.cs" and click "Add"

![capture31](https://cloud.githubusercontent.com/assets/10904957/23104891/4885be28-f711-11e6-9764-ebeee99b7199.PNG)

3.2 Add the sample properties for our login details

```html
public class UserDetailCredentials
{
   public int Id { get; set; }
   public string Username { get; set; }
   public string Password { get; set; }
   public string Name { get; set; }
}
```
3.3 Build the solution (Ctrl+Shift+B)
### 4.0 Adding our code snippet in our web service api controller

4.1 Go to your api web service controller and paste this following code snippet. In this code, we create a new HTTP Get Request with a custom route request of api/UserCredentials/username={username}/password={password}

```html
[HttpGet]
[Route("api/UserCredentials/username={username}/password={password}")]
public async Task<IHttpActionResult> UserDetailsLogin(string username, string password)
{
   UserDetailCredentials login =
                await db.UserDetailCredentials.Where(x => x.Username == username && x.Password == password).SingleOrDefaultAsync();
   if (login == null)
   {
      return NotFound();
   }
   return Ok(login);
}
```
### 5.0 Creating the Rest Client class

5.1 Right click the RestAPIClient Folder -> Add -> Class... In this example, we name our class model as "RestClient.cs" and click "Add"

![capture32](https://cloud.githubusercontent.com/assets/10904957/23104958/71a5fb14-f712-11e6-8aff-b7d8ca7a822f.PNG)

5.2 Inside the RestClient class, include this namespace

``` html
using System.Net.Http;
```

5.3 And paste this code snippet that I created.

``` html
public class RestClient<T>
{
   private const string MainWebServiceUrl = "http://MainHost/"; // Put your main host url here
   private const string LoginWebServiceUrl = MainWebServiceUrl + "api/UserCredentials/"; // put your api extension url/uri here

   // This code matches the HTTP Request that we included in our api controller
   public async Task<bool> checkLogin(string username, string password)
   {
      var httpClient = new HttpClient();

      var response = await httpClient.GetAsync(LoginWebServiceUrl + "username=" + username + "/" + "password=" + password);

      return response.IsSuccessStatusCode; // return either true or false
   }
}
```
