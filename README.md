# Dynamics-Nav-Extensions

## Undo item charges
Microsoft Dynamics Nav does not provide the option to undo shipped item charges out of the box. The standard procedure would be to invoice the charges and issue a credit memo.
This procedure is too cumbersome and inefficient for SWIM. 
1)	Unfortunately we have mandatory recycle fees in Belgium and these fees are linked to most of our items as item charges. As such it is not possible to easily undo a shipment. 
2)	SWIM make a lot of shipments that are often returned.

This codeunit is not 100% idiot proof, might contain errors and should be considered unsafe in a production environment.

### Installing and testing
Import the codeunit like usual and then make the following adjustments to “Page 131 Posted Sales Shpt. Subform”
1)	 Add a “page action” to the action group “functions”. My action looks like this:
```
      { 2029610 ;2   ;Action    ;
                      AccessByPermission=TableData 2000000053=D;
                      CaptionML=ENU=Undo Item Charge;
                      ApplicationArea=#Basic,#Suite;
                      Image=UnApply;
                      OnAction=BEGIN
                                 UndoItemCharge;
                               END;
                                }
```
This specific permission is generally not granted to regular users and is my way of restricting this function to them.

2)	Create a local function for the new codeunit: 
```
    LOCAL PROCEDURE UndoItemCharge@2029610();
    VAR
      SalesShptLine@1000 : Record 111;
      UserSetup@2029610 : Record 91;
    BEGIN
      SalesShptLine.COPY(Rec);
      CurrPage.SETSELECTIONFILTER(SalesShptLine);
      CODEUNIT.RUN(CODEUNIT::"Undo Item Charges",SalesShptLine);
    END;
```


## Donation

[![ko-fi](https://www.ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/T6T51XKUJ)

|Ethereum|Bitcoin|
|:-:	|:-:	|
|0x6b78d3deea258914C2f4e44054d22094107407e5|bc1qvvh8s3tt97cwy20mfdttpwqw0vgsrrceq8zkmw|
|![eth](https://raw.githubusercontent.com/Barabazs/Barabazs/master/.github/eth.png)|![btc](https://raw.githubusercontent.com/Barabazs/Barabazs/master/.github/btc.png)|
