# Technical Debt Analysis

Scan codebase for technical debt and improvement opportunities:

1. Find TODO, FIXME, HACK comments
2. Identify code duplication
3. Look for outdated dependencies
4. Find unused imports/variables
5. Identify complex functions that need refactoring
6. Generate prioritized improvement list
7. Identify areas where best practice can be applied, i.e. obeject-oriented approach

Use multiple subagents to research the code base. Document your plans to fix issues int tech_debt/tech_debt_repair.md 

Use as much time as you need.  

Create a report of your analysis in tech_debt/tech_debt_report.md

Then create a comprhensive and detailed remediation plan. The remediation plan should include a lot fo detail like specific files, line numbers, description of issue, and plan for remediation, etc. Remember, this application had not be released publicly and therefore not backwards functionality needs to be maintained. You can plan to implement improvements directly. Ensure that the plan takes not of how various aspects of the code works so that as systems are improved all functionality still works as intended. The plan should also contain cues to periodically run builds and fix build errors as they arise.

The remdiation plan should aim to resolve issues using subagents. Remember to include steps to remove dead and outdated code, and clean things up where possible. Before deciding on what code to remove, research extensively to understand how its used first. Make sure to use DRY principles, and reuse/update existing methods before trying to create new ones. Centralize logic.

Focus area: $ARGUMENTS (optional: specific directory/file, if not specified focuse on the entire project codebase)