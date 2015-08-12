quickbooks-python
=================

For base I took work of simonv3 and adopted it to my specific needs to use in my Django Rest Framework project. Decided to share project so that someone else might have benefit of it. In particular I have changed/added:

- Support for *sandbox* servers. Gets controled by **is_sandbox** property.
- Replace println statemes with python logging framework. For verbose output used DEBUG or INFO levels.
- 

Intuit now has a reporting API you can use so I deleted the makeshift ones I contributed.
