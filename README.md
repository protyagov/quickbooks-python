quickbooks-python
=================

This builds on the work of simonv3. I'm adding some functionality to handle additional accounting functions,
beginning with a programmatic export of a chart of accounts and a facility for generating ledger lines from transaction business objects (e.g. Bill, JournalEntry, Purchase).

I'm new to github, eager to build some cool things here, and welcome your constructive feedback on how to
improve my coding, collaboration, and knowledge base.

Generally when using this module (or any of the QBO v3 API wrappers out there), keep in mind that there are some glaring omissions in it's functionality that (AFAIK) no one is able to get around programmatically. For example, you can't access (or create, update, or delete, obvi) Deposits or Transfers.

Intuit now has a reporting API you can use so I deleted the makeshift ones I contributed.
