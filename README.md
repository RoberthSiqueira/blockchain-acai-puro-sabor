# Perishable Goods Network

> Example business network that shows manufacturers, shippers and retailers defining contracts for the price of perishable goods, based on temperature readings received for shipping containers.

The business network defines a contract between manufacturers and retailers. The contract stipulates that: On receipt of the shipment the retailer pays the manufacturer the unit price x the number of units in the shipment. Shipments that arrive late are free. Shipments that have breached the low temperate threshold have a penalty applied proportional to the magnitude of the breach x a penalty factor. Shipments that have breached the high temperate threshold have a penalty applied proportional to the magnitude of the breach x a penalty factor.

This business network defines:

**Participants**
`Manufacturer` `Retailer` `Shipper`

**Assets**
`Contract` `Shipment`

**Transactions**
`TemperatureReading` `ShipmentReceived` `GeraLaudo`

To test this Business Network Definition in the **Test** tab:

Submit a `GeraLaudo` transaction:

```
{
  "$class": "org.acme.shipping.perishable.GeraLaudo"
}
```

This transaction populates the Participant Registries with a `Manufacturer`, an `Retailer` and a `Shipper`. The Asset Registries will have a `Contract` asset and a `Shipment` asset.

Submit a `TemperatureReading` transaction:

```
{
  "$class": "org.acme.shipping.perishable.TemperatureReading",
  "centigrade": 8,
  "shipment": "resource:org.acme.shipping.perishable.Shipment#SHIP_001"
}
```

If the temperature reading falls outside the min/max range of the contract, the price received by the manufacturer will be reduced. You may submit several readings if you wish. Each reading will be aggregated within `SHIP_001` Shipment Asset Registry.

Submit a `ShipmentReceived` transaction for `SHIP_001` to trigger the payout to the manufacturer, based on the parameters of the `CON_001` contract:

```
{
  "$class": "org.acme.shipping.perishable.ShipmentReceived",
  "shipment": "resource:org.acme.shipping.perishable.Shipment#SHIP_001"
}
```

If the date-time of the `ShipmentReceived` transaction is after the `arrivalDateTime` on `CON_001` then the manufacturer will no receive any payment for the shipment.

Congratulations!
