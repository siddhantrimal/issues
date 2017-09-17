#### Roadblocks

1. _OPEN: Issue003_
    * Following Issue002, further problems were encountered as a part of it. This problem is encountered:
    ![A datatype mismatch was found while making a query](screenshots/03.png)

        This was resolved as:
        ```csharp
        [Column(TypeName = "DateTime2")]
        public DateTime AddedDate { get; set; }
        ```

        But then it produced this following error:

        ![Now the Database wasn't updates wrt the context](screenshots/04.png)

        The Stack-Trace is as follows:
        ```
        [InvalidOperationException: The model backing the 'DynamicFormDbContext' context has changed since the database was created. Consider using Code First Migrations to update the database (http://go.microsoft.com/fwlink/?LinkId=238269).]
        System.Data.Entity.CreateDatabaseIfNotExists`1.InitializeDatabase(TContext context) +3555561
        System.Data.Entity.Internal.<>c__DisplayClassf`1.<CreateInitializationAction>b__e() +76
        System.Data.Entity.Internal.InternalContext.PerformInitializationAction(Action action) +60
        System.Data.Entity.Internal.InternalContext.PerformDatabaseInitialization() +395
        System.Data.Entity.Internal.LazyInternalContext.<InitializeDatabase>b__4(InternalContext c) +11
        System.Data.Entity.Internal.RetryAction`1.PerformAction(TInput input) +110
        System.Data.Entity.Internal.LazyInternalContext.InitializeDatabaseAction(Action`1 action) +214
        System.Data.Entity.Internal.LazyInternalContext.InitializeDatabase() +97
        System.Data.Entity.Internal.InternalContext.GetEntitySetAndBaseTypeForType(Type entityType) +28
        System.Data.Entity.Internal.Linq.InternalSet`1.Initialize() +53
        System.Data.Entity.Internal.Linq.InternalSet`1.get_InternalContext() +16
        System.Data.Entity.Internal.Linq.InternalSet`1.ActOnSet(Action action, EntityState newState, Object entity, String methodName) +66
        System.Data.Entity.Internal.Linq.InternalSet`1.Add(Object entity) +110
        System.Data.Entity.DbSet`1.Add(TEntity entity) +83
        DynamicForm.Self.Areas.Admin.Controllers.FormController.Create(FormViewModel form) in D:\SoAni\FormManipulation\DynamicForm.Self\DynamicForm.Self\Areas\Admin\Controllers\FormController.cs:55
        lambda_method(Closure , ControllerBase , Object[] ) +103
        System.Web.Mvc.ActionMethodDispatcher.Execute(ControllerBase controller, Object[] parameters) +14
        System.Web.Mvc.ReflectedActionDescriptor.Execute(ControllerContext controllerContext, IDictionary`2 parameters) +157
        System.Web.Mvc.ControllerActionInvoker.InvokeActionMethod(ControllerContext controllerContext, ActionDescriptor actionDescriptor, IDictionary`2 parameters) +27
        System.Web.Mvc.Async.AsyncControllerActionInvoker.<BeginInvokeSynchronousActionMethod>b__39(IAsyncResult asyncResult, ActionInvocation innerInvokeState) +22
        System.Web.Mvc.Async.WrappedAsyncResult`2.CallEndDelegate(IAsyncResult asyncResult) +29
        System.Web.Mvc.Async.WrappedAsyncResultBase`1.End() +49
        System.Web.Mvc.Async.AsyncControllerActionInvoker.EndInvokeActionMethod(IAsyncResult asyncResult) +32
        System.Web.Mvc.Async.AsyncInvocationWithFilters.<InvokeActionMethodFilterAsynchronouslyRecursive>b__3d() +50
        System.Web.Mvc.Async.<>c__DisplayClass46.<InvokeActionMethodFilterAsynchronouslyRecursive>b__3f() +228
        System.Web.Mvc.Async.<>c__DisplayClass33.<BeginInvokeActionMethodWithFilters>b__32(IAsyncResult asyncResult) +10
        System.Web.Mvc.Async.WrappedAsyncResult`1.CallEndDelegate(IAsyncResult asyncResult) +10
        System.Web.Mvc.Async.WrappedAsyncResultBase`1.End() +49
        System.Web.Mvc.Async.AsyncControllerActionInvoker.EndInvokeActionMethodWithFilters(IAsyncResult asyncResult) +34
        System.Web.Mvc.Async.<>c__DisplayClass2b.<BeginInvokeAction>b__1c() +26
        System.Web.Mvc.Async.<>c__DisplayClass21.<BeginInvokeAction>b__1e(IAsyncResult asyncResult) +100
        System.Web.Mvc.Async.WrappedAsyncResult`1.CallEndDelegate(IAsyncResult asyncResult) +10
        System.Web.Mvc.Async.WrappedAsyncResultBase`1.End() +49
        System.Web.Mvc.Async.AsyncControllerActionInvoker.EndInvokeAction(IAsyncResult asyncResult) +27
        System.Web.Mvc.Controller.<BeginExecuteCore>b__1d(IAsyncResult asyncResult, ExecuteCoreState innerState) +13
        System.Web.Mvc.Async.WrappedAsyncVoid`1.CallEndDelegate(IAsyncResult asyncResult) +29
        System.Web.Mvc.Async.WrappedAsyncResultBase`1.End() +49
        System.Web.Mvc.Controller.EndExecuteCore(IAsyncResult asyncResult) +36
        System.Web.Mvc.Controller.<BeginExecute>b__15(IAsyncResult asyncResult, Controller controller) +12
        System.Web.Mvc.Async.WrappedAsyncVoid`1.CallEndDelegate(IAsyncResult asyncResult) +22
        System.Web.Mvc.Async.WrappedAsyncResultBase`1.End() +49
        System.Web.Mvc.Controller.EndExecute(IAsyncResult asyncResult) +26
        System.Web.Mvc.Controller.System.Web.Mvc.Async.IAsyncController.EndExecute(IAsyncResult asyncResult) +10
        System.Web.Mvc.MvcHandler.<BeginProcessRequest>b__5(IAsyncResult asyncResult, ProcessRequestState innerState) +21
        System.Web.Mvc.Async.WrappedAsyncVoid`1.CallEndDelegate(IAsyncResult asyncResult) +29
        System.Web.Mvc.Async.WrappedAsyncResultBase`1.End() +49
        System.Web.Mvc.MvcHandler.EndProcessRequest(IAsyncResult asyncResult) +28
        System.Web.Mvc.MvcHandler.System.Web.IHttpAsyncHandler.EndProcessRequest(IAsyncResult result) +9
        System.Web.CallHandlerExecutionStep.System.Web.HttpApplication.IExecutionStep.Execute() +9986301
        System.Web.HttpApplication.ExecuteStep(IExecutionStep step, Boolean& completedSynchronously) +155
        ```

        To resolve this, I did the following

        ```
        Update-Database -TargetMigration $InitialDatabase -Force
        ```
        because `Update-Database` was not being accepted with the following message:
        ```
        PM> Update-Database
        Specify the '-Verbose' flag to view the SQL statements being applied to the target database.
        No pending explicit migrations.
        Unable to update database to match the current model because there are pending changes and automatic migration is disabled. Either write the pending model changes to a code-based migration or enable automatic migration. Set DbMigrationsConfiguration.AutomaticMigrationsEnabled to true to enable automatic migration.
        You can use the Add-Migration command to write the pending model changes to a code-based migration.
        ```
        It completely deleted all my older FormField data. That was unfortunate.

        The next build with `Add-Migration InitialCreate` was also unsuccessful in ultimately solving the problem. 
        
        The Webapp finally worked at `http://localhost:51970/Admin/Form/Create` but I still need to ditch this crazy workaround.
        ![The Webapp finally works](screenshots/05.png)

        ![Webapp fetches data from the backend](screenshots/06.png)

        This Issue003 shall remain _OPEN_ because I need to remember to solve this issue.




1. _OPEN: Issue002_

    * Issue is: After executing the first `Update-Database`, I can't seem to change the value of a column as `nullable` from VS2017 as the `Update-Database` command does not respond. The only workaround I was able to find was deleting the entire `InitialCreate` migration and then running the `Update-Database` command. I'm afraid this might cause problems in the future.

        Updating the database still produces the `No pending explicit migrations` response:
        ```
        PM> Update-Database
        Specify the '-Verbose' flag to view the SQL statements being applied to the target database.
        No pending explicit migrations.
        Running Seed method.
        ```
    
    I am trying to Update the database with Code First method but, once created during Initial, it just won't update.

    After doing `Add-Migration InitialCreate` folowed by `Update-Database` , a database is created as follows:

    ![A Table such as this is created automatically when the migration is run](screenshots/02.png)


1. __CLOSED: Issue001__ 
    * Issue has been fixed. Turns out I had been using the EF 6.x Datacontext generator, which is commonly known to produce these results. I fixed this by using a normal cs class and coding it manually.

    I am not sure why, but I'm getting this error while adding DbContext to my project's Model. If I ignore the error, it does not get built properly as a `.cs` file.  _9-15-2017 12:20PM_
    ![Getting error when adding DbContext; strange](screenshots/01.png)
    