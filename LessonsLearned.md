# Lessons Learned

Documenting all obstacles encountered and how they were solved.

Oct 2 2024

1. I couldn't connect to the local_postgres docker container due to password authentication on the admin user. The problem was that the password that I was entering into the docker run bash script was failing the default password policy. Changing the password to be longer and with other symbols and numbers made it work.

2. When trying to switch to npgsql (EF Core provider for PostgreSQL), I was running into the error that the user does not have permissions to create a database, so I switched back to using the admin user. (so TODO fix this in future)

3. When trying to switch to npgsql, I also got the error of MS SQL Server types appearing in the create table query (nvarchar). This was because I had manually configured column types inside Infrastructure.EntitiesConfiguration using NVARCHAR.

Oct 11 2024

1. When trying to use absolute paths in a Vite TypeScript project using Vite resolve alias, make sure you update the correct compilerOptions so the linter knows about the aliases. In Vite TypeScript projects the default tsconfig.json references two projects, one for the app (tsconfig.app.json; edit the paths for that one), and one for Vite (tsconfig.node.json).

Oct 12 2024

1. When trying to Include navigation references, if the navigation reference is configured as IsRequired(), and it is found to be null, then that entity with a null navigation reference is thrown out. Make sure if the navigation reference is allowed to be null, that IsRequired(false) is used.

Oct 24 2024

1. TypeScript Object.values(object_name) gets the values of the members in an object. Object.keys(object_name) gets the names of the members. Useful for checking Enums.

2. In React useState, there is the functional form of setState. setState( prevValue => [...prevValue, newAppend]); This makes sure that the current value is used for prevValue.

Oct 25 2024

1. SignalR is already included in ASP.NET Core. Do not use the Microsoft.ASPNET.SignalR (not asp.net core).

Oct 26 2024

1. In ASP.NET, inside a BackgroundService, when it calls a DI'ed service, error handling kinda falls apart. Such as null references. I think the thread just stops without warning. This can also be recognized if the in one test iteration a print statement works fine, but after you try the null reference print statement before it, then it stops (as it exited the function before the print statement that was working before). In this case, I had to dbContext.Include(x => User) which I was using in the Project entity.

2. In React, remember that callbacks get a stale state, so you have to useRef, useref.current = state. And then in the callback, useRef.current to get the state.

Oct 27 2024

1. My generic query handler returns a list of TOutput. When it got to the TypeScript side, I used my apiResponse<Project> but it was actually a Project[], so the linter was showing the wrong type because I didn't realize what the actual API response was.

Oct 31 2024

1. When using CORS, not all headers are sent over. This was a problem when trying to read the Content-Disposition header. On the server-side append Access-Control-Expose-Headers header with the header to include.

Nov 22 2024

1. When creating a Dto on the API server side, make sure your when you Include a navigation reference, to also use a Dto. Or else you will get object cycle errors.

