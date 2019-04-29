---?color=linear-gradient(to right, #66ccff, #e6f7ff)
@title[Introduction]

@snap[west text-25 text-bold text-white]
Open Day<br>*April 2019*
@snapend

@snap[south-west byline text-white text-06]
By Deepesh Garg
@snapend

---
@snap[north-west]
Partner Portal post release Activities
@snapend

@snap[west list-content-concise span-100]
@ol[list-bullets-black](false)
- Image upload button fix in partner profile webfrom
- Minor changes and fixes in partne listing page and plans page
- Partner credit balance report
- Changes in signup form
- Remove old reseller pages
@olend
<br><br>
@snapend

---
#### Merge Lead and Customer field into a Dynamic Field for Quotation and Opportunity

![RIGPL](assets/img/rigpl.png)

---

##### New Inactive Items Report

![Report](assets/img/inactive.png)


---


@snap[north-west]
[ICP India] Notification of Receipt against Material Request
@snapend


---


#### E-way bill json creation creation and download

![EWB](assets/img/ewb.gif)

---


### Sample JSON

```json
	{
    "billLists": [
        {
            "OthValue": 0,
            "TotNonAdvolVal": 0,
            "actualFromStateCode": 27,
            "actualToStateCode": 27,
            "cessValue": 0,
            "cgstValue": 0,
            "docDate": "27/04/2019",
            "docNo": "SINV-00186",
            "docType": "INV",
            "fromAddr1": "23/D, Paper Box Industrial Estate",
            "fromAddr2": "MC Road, Andheri East",
            "fromGstin": "27AAECE4835E1ZR",
            "fromPincode": 401105,
            "fromPlace": "Mumbai",
            "fromStateCode": 27,
            "fromTrdName": "Gadget Technologies Pvt. Ltd.",
            "igstValue": 0,
            "itemList": [
                {
                    "cessNonAdvol": 0,
                    "cessRate": 0,
                    "cgstRate": 0,
                    "hsnCode": 8517,
                    "igstRate": 0,
                    "qtyUnit": "",
                    "sgstRate": 18.0,
                    "taxableAmount": 48000.0
                },
                {
                    "cessNonAdvol": 0,
                    "cessRate": 0,
                    "cgstRate": 0,
                    "hsnCode": 998891,
                    "igstRate": 0,
                    "qtyUnit": "",
                    "sgstRate": 18.0,
                    "taxableAmount": 0.0
                }
            ],
            "sgstValue": 8640.0,
            "subSupplyType": 1,
            "supplyType": "O",
            "toAddr1": "56",
            "toAddr2": "Cliff Hills",
            "toGstin": "URP",
            "toPincode": 400050,
            "toPlace": "Mumbai",
            "toStateCode": 27,
            "toTrdName": "Latte Solutions",
            "totInvValue": 56640.0,
            "totalValue": 48000.0,
            "transDistance": 2000.0,
            "transDocDate": "27/04/2019",
            "transDocNo": "",
            "transMode": 1,
            "transType": 1,
            "transporterId": "",
            "transporterName": "",
            "userGstin": "27AAECE4835E1ZR",
            "vehicleNo": "KA12KA1234",
            "vehicleType": "R"
        }
    ],
    "version": "1.0.1118"
}
```


---


##### Addition of custom fields to Script and Query reports

![Column](assets/img/column.gif)



---


##### [Wallbox] Automatic PO - SO Creation in Inter-company Transactions

![Inter](assets/img/automatic.gif)



---

@snap[north-west]
Support Issues + Bug Fixes
@snapend

```python
	# Patch for sapcon to repost stock enteries
	import frappe
	from erpnext.stock.stock_balance import repost_stock
	from erpnext.stock.utils import get_bin

	def execute():

		item_set = set()

		for table in ["Sales Order", "Purchase Order", "Material Request", "Work Order"]:
			warehouse = 'i.source_warehouse' if table == 'Work Order' else 'i.warehouse'
			item_details = frappe.db.sql("""select i.item_code, {warehouse} as warehouse
				from `tab{table} Item` i, `tab{table}` s where i.parent = s.name and
				s.docstatus = 1 """.format(table = table, warehouse = warehouse), as_dict=1)

			for item in item_details:
				item_set.add((item.item_code, item.warehouse))

		sle = frappe.db.sql("select item_code, warehouse from `tabStock Ledger Entry` where docstatus = 1", as_dict=1)

		for items in sle:
			item_set.add((item.item_code, item.warehouse))

		packed_items = frappe.db.sql("select item_code, warehouse from `tabPacked Item` ", as_dict=1)

		for items in packed_items:
			item_set.add((item.item_code, item.warehouse))


		for item in item_set:
			repost_stock(item[0], item[1])
			bin_obj = get_bin(item[0], item[1])
			bin_obj.update_reserved_qty_for_production()
			bin_obj.update_reserved_qty_for_sub_contracting()
```

---
@snap[north-west]
@snapend

@snap[west list-content-concise span-120]
@ol[list-bullets-black](false)
- RTL scrolling issue fix in datatable (Bug Sprint meetup)
- Query Report not being reloaded when route options are passed
- Custom doctype remaning issue fix
- Stock ledger report issue fix when warehouse filter is applied
- Minor fixes in GSTR-3b Report
- Datatype fix balance(account currency) in General Ledger (v10.x.x)
- GSTR-1 B2C Small Report issue fix
- Party type validation in Payment Entry
- TDS payable monthly report fix
@olend
<br><br>
@snapend





