FOR USER: Replace the project mapping step 5 with your own project mappings. Follows the format: \\\[\\\['Project 1 name in Toggl', 'Project 1 name in Toggl'], \\\[...]]. For unassigned hours in Toggl, use null, as seen in step 5.

Do not include this in Claude prompt.



Instructions:



\*\*CRITICAL: Do not include any comments, analyis or summary of the information. The only thing on screen should be the tools being called and the artifact with the report.\*\*



\*\*CRITICAL: Follow these steps explicitly, do not take shortcuts\*\*:



1\. Use list\_entries to obtain time entries from Harvest. Skip this step if comparing two Toggl workspaces. Use multiple times if required.



2\. Use search\_time\_entries\_detailed\_report to obtain time entries from Toggl. Skip this step if comparing two Harvest workspaces. Use multiple times if required.



3\. Use get\_all\_projects to obtain projects from Toggl.



4\. Use get\_project\_users to learn which users worked on which projects in Toggl.



5\. Use the following project mapping: \[\['Project 1'], \['Project 2'], \['Project 3'], \[null, 'Personal Development']].



6\. If comparing two Toggl workspaces, use the format \[0, 0], two Harvest accounts \[1, 1], and a Toggl and Harvest account \[0, 1].



7\. Use compare\_entries to obtain both system hour totals and anomalous time entries.



8\. Use information from get\_project\_users to map users to which projects they worked on.



9\. Check that time entries are attributed to the correct user project and system by checking the user ID, project ID, and system of each time entry.



10\. \*\*CRITICAL: Ensure that all time entries are accounted for and correctly assigned. DO NOT move to the next step unless this has been done.\*\*



11\. When generating a daily report ensure that all users assigned to a project are included in that project's section, even if they didn't log any hours. DO NOT include users that are not assigned to a project in that project's section. Users that are not assigned to a project based on information from get\_project\_users should not be included in that project's section.



12\. When generating a daily report, include ALL time entries from either system, even if a project does not have a corresponding project in another system.



13\. \*\*CRITICAL: DO NOT under any circumstances perform your own analysis, use exclusively information from compare\_toggl\_harvest\_entries to find discrepancies and come up with system total hours.\*\*



14\. Generate a daily report for the given date range using the following format.



Format:



Follow the following format explicitly, only allowing changes when replacing placeholders (System 1, User 1, M/D/Y, #, etc.). Do not include an additional sections (introduction, conclusion, key findings, etc.). If two differently named projects map together, include both project names in that section's header:

&nbsp;   

\### Time Entries by Project



\### Project 1 OR Project 1/Project A (Replace this with project name/Names of mapped projects.)



\### User 1 (Replace this with user name.)



\### System 1 (Replace this with system name.)

| Date | Hours |

|---------|-------------|

| M/D/Y | # hour(s) |

| M/D/Y | # hour(s) |

| Total | # hour(s) |



\### System 2

| Date | Hours |

|---------|-------------|

| M/D/Y | # hour(s) |

| M/D/Y | # hour(s) |

| Total | # hour(s) |



\### Discrepancies

| Date | Hours in System 1 | Hours in System 2 | Discrepancy |

|---------|-------------|-----------|---------|

| M/D/Y | # hour(s) | # hour(s) | \*\*+/- # hour(s)\*\*

| M/D/Y | # hour(s) | # hour(s) | \*\*+/- # hour(s)\*\*



(If no discrepancies, use "No discrepancies found.")



\### User 2



\### System 1

| Date | Hours |

|---------|-------------|

| M/D/Y | # hour(s) |

| M/D/Y | # hour(s) |

| Total | # hour(s) |



\### System 2

| Date | Hours |

|---------|-------------|

| M/D/Y | # hour(s) |

| M/D/Y | # hour(s) |

| Total | # hour(s) |



\### Discrepancies

| Date | Hours in System 1 | Hours in System 2 | Discrepancy |

|---------|-------------|-----------|---------|

| M/D/Y | # hour(s) | # hour(s) | \*\*+/- # hour(s)\*\*

| M/D/Y | # hour(s) | # hour(s) | \*\*+/- # hour(s)\*\*



(If no discrepancies, use "No discrepancies found.")



\### Project 2 OR Project 2/Project B (Replace this with project name/Names of mapped projects.)



\### User 1



\### System 1

| Date | Hours |

|---------|-------------|

| M/D/Y | # hour(s) |

| M/D/Y | # hour(s) |

| Total | # hour(s) |



\### System 2

| Date | Hours |

|---------|-------------|

| M/D/Y | # hour(s) |

| M/D/Y | # hour(s) |

| Total | # hour(s) |



\### Discrepancies

| Date | Hours in System 1 | Hours in System 2 | Discrepancy |

|---------|-------------|-----------|---------|

| M/D/Y | # hour(s) | # hour(s) | \*\*+/- # hour(s)\*\*

| M/D/Y | # hour(s) | # hour(s) | \*\*+/- # hour(s)\*\*



(If no discrepancies, use "No discrepancies found.")



\### User 2



\### System 1

| Date | Hours |

|---------|-------------|

| M/D/Y | # hour(s) |

| M/D/Y | # hour(s) |

| Total | # hour(s) |



\### System 2

| Date | Hours |

|---------|-------------|

| M/D/Y | # hour(s) |

| M/D/Y | # hour(s) |

| Total | # hour(s) |



\### Discrepancies

| Date | Hours in System 1 | Hours in System 2 | Discrepancy |

|---------|-------------|-----------|---------|

| M/D/Y | # hour(s) | # hour(s) | \*\*+/- # hour(s)\*\*

| M/D/Y | # hour(s) | # hour(s) | \*\*+/- # hour(s)\*\*



(If no discrepancies, use "No discrepancies found.")



\### Summary

Total Hours by System:

| System | Hours |

|---------|-------------|

| System 1 | # hour(s) |

| System 2 | # hour(s) |

| Difference | +/- # hour(s) |

