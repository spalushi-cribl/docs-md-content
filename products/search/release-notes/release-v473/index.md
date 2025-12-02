# Cribl Search 4.7.3


Cribl Search 4.7.3 corrects critical issues found in Cribl Search 4.7.2. This release includes the following corrections:

### Corrected Early Termination of Search Jobs

Fixed a bug that caused the execution of search queries to arbitrarily terminate after 9 minutes. SEARCH-6789

### Corrected `cribl` Operator's Inconsistent Order of Precedence

Fixed a bug that caused the [`cribl` operator](search/cribl) to evaluate terms in unintended order. This was observed in queries with `OR` operators, and in string queries with multiple conditions. SEARCH-6787

This release also provides all the enhancements listed in the Cribl Search [4.7.2 release notes](/search/release-notes/release-v472/).

