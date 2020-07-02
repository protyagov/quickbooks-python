quickbooks-python
=================

For base I took work of simonv3 and adopted it to my specific needs to use in my Django Rest Framework project. Decided to share project so that someone else might have benefit of it. In particular I have changed/added:

- Support for *sandbox* servers. Gets controled by **is_sandbox** property.
- Replace println statemes with python logging framework. For verbose output used DEBUG or INFO levels.



Intuit now has a reporting API you can use so I deleted the makeshift ones I contributed.

**Example**


```python
qb = QuickBooks(consumer_key=settings.QB_OAUTH_CONSUMER_KEY,
                consumer_secret=settings.QB_OAUTH_CONSUMER_SECRET,
                access_token=request.user.client.accounting['access_token'],
                access_token_secret=request.user.client.accounting['access_token_secret'],
                company_id=request.user.client.accounting['realm_id'],
                is_sandbox=settings.QB_IS_SANDBOX)

# check if Account object exists
qb.is_object('Account', "where Name = '%s' " % account['Name'])

# check if Item object exists
qb.is_object('Item', "where Name = '%s'" % item['Name'])

# create Item object
qb.create_object('Item', item)

# get objects
res = qb.get_objects(qb_object, query_tail='<your query>')

```



**Create Invoices (we interface QuickBooks to Django ORM)**


```python
# Invoice already exists in QB, nothing to do.
invoice = Invoice.objects.get(pk=invoice_id)
if invoice.acc_invoice_id and self.qb.is_object(self.qb_object, "where Id='%s'" % invoice.acc_invoice_id):
    return

# Ensure owner for this invoice exists in QB
AccountingOwnerView(request=self.request).create_qb_object(invoice.owner.pk)

# Nothing to send, just return.
invoice_lines = [x for x in InvoiceLine.objects.filter(parent=invoice)]
if len(invoice_lines) == 0:
    self.error.append('Invoice "%s" is skipped because it has no invoice lines.' % invoice.name)
    return

# Get or create items accounts
logger.debug('QB: Create Accounts')
self.qb_accounts = {'AssetAccountRef': self._create_qb_account(self.a_account),
                    'IncomeAccountRef': self._create_qb_account(self.i_account),
                    'ExpenseAccountRef': self._create_qb_account(self.e_account)
                    }

# Ensure QB has item for each task and material from our system
self._create_qb_items(invoice_lines)

# Create invoice on QB system
logger.debug('QB: Create Invoice')
qb_invoice = self.qb.create_object(self.qb_object, AccountingInvoiceSerializer(invoice_lines, many=True).data)
if qb_invoice:
    invoice.acc_invoice_id = qb_invoice['Id']
    invoice.status = InvoiceStatus.SENT
    invoice.save()
elif self.qb.error:
    self.error.append('%s: %s' % (invoice_lines[0].parent.name, self.qb.error))

```
