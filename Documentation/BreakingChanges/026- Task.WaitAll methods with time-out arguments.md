## 26: Task.WaitAll methods with time-out arguments

### Scope
Minor

### Version Introduced
4.5

### Change Description
Task.WaitAll behavior was made more consistent in .NET 4.5. 

In the .NET Framework 4, these methods behaved inconsistently. When the time-out expired, if one or more tasks were completed or canceled before the method call, the method threw an AggregateException exception. When the time-out expired, if no tasks were completed or canceled before the method call, but one or more tasks entered these states after the method call, the method returned false.<br/><br/>In the .NET Framework 4.5, these method overloads now return false if any tasks are still running when the time-out interval expired, and they throw an AggregateException exception only if an input task was cancelled (regardless of whether it was before or after the method call) and no other tasks are still running.

- [ ] Quirked
- [ ] Build-time break
- [ ] Source analyzer available

### Recommended Action
If an AggregateException was being caught as a means of detecting a task that was cancelled prior to the WaitAll call being invoked, that code should instead do the same detection via the IsCanceled property (for example: .Any(t =&gt; t.IsCanceled)) since .NET 4.6 will only throw in that case if all awaited tasks are completed prior to the timeout.

### Affected APIs
* M:System.Threading.Tasks.Task.WaitAll(System.Threading.Tasks.Task[],System.Int32)
* M:System.Threading.Tasks.Task.WaitAll(System.Threading.Tasks.Task[],System.Int32,System.Threading.CancellationToken)
* M:System.Threading.Tasks.Task.WaitAll(System.Threading.Tasks.Task[],System.TimeSpan)

[More information](https://msdn.microsoft.com/en-us/library/hh367887\(v=vs.110\).aspx#core)

<!--
    ### Notes
    Source analyzer status: Pri 2, Done
-->


