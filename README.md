SetupDiag
Introduction

SetupDiag is a Microsoft Windows diagnostic utility designed to identify and analyze the causes of failed Windows updates, feature upgrades, and operating system installations. It helps IT administrators, system engineers, and support specialists troubleshoot Windows upgrade errors by examining installation logs and matching detected problems against a set of predefined diagnostic rules. The tool supports Windows 10 and Windows 11 environments and provides detailed information about installation failures, compatibility issues, driver problems, and unexpected system rollbacks.

The primary purpose of SetupDiag is to simplify Windows upgrade troubleshooting by identifying the underlying reasons for installation failures. Instead of manually reviewing thousands of lines in Windows Setup log files, administrators can use automated log analysis to identify relevant error codes, failed installation phases, and potential causes of unsuccessful upgrades.

SetupDiag supports both online and offline diagnostic operations. Online analysis examines Windows Setup logs on the current computer, while offline analysis processes previously collected log files from another system. This capability is particularly useful for diagnosing remote computers, investigating deployment failures, and performing centralized troubleshooting in enterprise IT environments.

The utility generates diagnostic reports containing information about detected failures, matched rules, error codes, and relevant log entries. Results can be saved in text, XML, or JSON formats, depending on the selected configuration. SetupDiag can also create compressed archives containing the analyzed logs and diagnostic results.

By integrating log analysis into Windows deployment troubleshooting workflows, administrators can identify upgrade blockers, investigate installation failures, and determine appropriate corrective actions before attempting another Windows upgrade.

How to Use SetupDiag to Diagnose Windows Upgrade Errors

SetupDiag can be executed manually from an elevated Windows Command Prompt or automatically by Windows Setup when an installation failure occurs. Before starting an investigation, administrators should ensure that they are using a current version of the utility and have sufficient permissions to access the relevant installation logs.

For a standard online diagnostic session, run:

cmd
SetupDiag.exe /Output:C:\SetupDiag\Results.log


In this configuration, the utility automatically locates Windows Setup logs on the current computer, analyzes available diagnostic information, and saves the results to the specified file. If the output directory does not exist, create it before execution.

To investigate a failed upgrade on another computer, collect the relevant Windows Setup log directories and copy them to an accessible location. These logs commonly include the Panther and Rollback directories under $Windows.~BT\Sources, as well as Windows\Panther.

Run offline analysis using:

cmd
SetupDiag.exe /LogsPath:C:\SetupDiag\CollectedLogs /Output:C:\SetupDiag\OfflineResults.log


The /LogsPath parameter specifies the directory containing the collected logs. SetupDiag automatically searches its subdirectories, allowing administrators to analyze complete log collections without manually selecting individual files.

The /Output parameter controls the diagnostic report location. Additional options include /Format:xml or /Format:json for structured output and /ZipLogs:True for creating an archive containing the analyzed logs and results.

After execution, review the matched diagnostic rules, failure descriptions, and error information. When multiple failures are reported, the last failure is often the most important because it may represent the fatal error that triggered the rollback.

SetupDiag Command-Line Parameters and Diagnostic Results

SetupDiag provides command-line parameters that allow IT professionals to customize Windows upgrade log analysis, control report generation, and integrate diagnostics into troubleshooting procedures. Understanding these options is particularly important when investigating recurring installation failures across multiple computers.

The /LogsPath parameter enables offline analysis of previously collected Windows Setup logs. Specifying this parameter automatically switches the utility to offline mode, making the older /Mode parameter unnecessary. This is useful when the original computer is unavailable or its operating system cannot start normally.

The /Output parameter defines the full path and filename of the diagnostic report. If omitted, SetupDiag creates SetupDiagResults.log in its execution directory. Paths containing spaces must be enclosed in quotation marks.

The /Format parameter supports XML and JSON report generation. Structured formats are useful for automated processing, extracting diagnostic information, and incorporating upgrade failure analysis into enterprise reporting systems. Text output is used by default.

The /ZipLogs parameter controls whether SetupDiag creates a compressed archive containing the processed log files and diagnostic results. This is particularly useful when transferring troubleshooting data between support teams.

For example, the following command generates an XML report from collected installation logs:

cmd
SetupDiag.exe /LogsPath:C:\SetupDiag\Logs /Output:C:\SetupDiag\Results.xml /Format:xml


For Windows reset and recovery investigations, use /Scenario:Recovery to analyze recovery-related logs instead of standard upgrade logs.

Diagnostic results can also be examined in the Windows Registry. During manual online execution, the default location is HKLM\SYSTEM\Setup\MoSetup\Volatile\SetupDiag. Windows Setup uses a separate registry location when launching the utility automatically.

SetupDiag identifies potential failure causes rather than automatically repairing them. Administrators should correlate its findings with Windows Setup logs, driver information, and system configuration before applying corrective actions.
