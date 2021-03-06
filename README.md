# IOS-sdk
This is the velocity IOS SDK implementation. <br/>
It has the implementation of all the transaction payment solution methods for a merchant application who wants to access the Velocity payment gateway. <br/>

At the center of this SDK, there is the class <b>VelocityProcessor</b>. <br/>

The signature for initialising this class is as below: <br/>

<b> - (VelocityProcessor *) initWith:(NSString *)identityToken forAppProfileId:(NSString *)appProfileId forMerchantProfileId:(NSString *)merchantProfileId forWorkflowId:(NSString *)workflowId andSessionToken:(NSString *)sessiontoken andType:(BOOL )isTestAccount; </b> <br/>

@parameter  <b>sessionToken </b> - initialises the value for session token.  <br/>
@parameter  <b>identityToken </b> - initialises the value for identity token.  <br/>
@parameter  <b>appProfileId </b> - initialises the value for application profile Id.  <br/>
@parameter  <b>merchantProfileId </b> - initialises the value for merchant profile Id.  <br/>
@parameter  <b>workFlowId </b> - initializes the value for workflow Id.  <br/>
@parameter  <b>isTestAccount </b> - works as a flag for the TestAccount.  <br/>

Used in Sample App as <br/>

<b> velocityProcessorObj= [[VelocityProcessor alloc] initWith:kIdentityToken forAppProfileId:kAppProfileId forMerchantProfileId:kMerchantProfileId forWorkflowId:KWorkflowId andSessionToken:nil andType:kisTestAccountBOOL ]; </b> <br/>

<h2>1. VelocityProcessor </h2><br/>

This class provides the implementation of the following methods: <br/>
     1. createCardToken;   <br/>
     2. authorise;<br/>
     3. authorizeAndCapture;<br/>
     4. capture;<br/>
     5. undo;<br/>
     6. adjust;<br/>
     7. returnById;<br/>
     8.returnUnlinked;<br/>
     9.queryTransactionsDetail;<br/>
     10.captureAll;<br/>
     
     
<b> The class also provides delegate for conveying success and error response </b> <br/>

<h2>VelocityProcessor Delegates</h2><br/>
1.-(void)VelocityProcessorFinishedWithSuccess:(id )successAny;   //delegate method for successful transaction<br/>
2.-(void)VelocityProcessorFailedWithErrorMessage:(id )failedAny;   //delegate method for Failed transaction<br/>

<h2>1. createCardToken(....) </h2><br/>
The method is responsible for the invocation of verify operation on the Velocity REST server.<br/>
 <b>velocityPaymentTransaction Modal Class</b> - holds the values for the verify request VelocityPaymentTransaction <br/>
 
               		1.cardType - String     <br/>
			2.cardholderName - String     <br/>
               		3.panNumber-String   <br/>
        		4.expiryDate - String   <br/>
			5.street - String   <br/>
               		6.stateProvince - String     <br/>
               		7.postalCode - String   <br/>
               		8.phone - String    <br/>
			9.state - String     <br/>
               		10.cvDataProvided - String    <br/>
               		11.cVData - String   <br/>
	       		12.amount - String       <br/>
               		13.currencyCode - String       <br/> 
               		14.customerPresent - String     <br/>
               		15.employeeId - String     <br/>
               		16.entryMode - String      <br/>
               		17.industryType - String   <br/>
               		18.email - String   <br/>
               		19.transactionDateTime - String   <br/>
               		20.city -String <br/>
  
<h2>How to set the Ui value on VelocityPaymentTransaction model </h2><br/>
<b>Sample code</b><br/> 

	    VelocityPaymentTransaction *vPTMCObj;  //velocityProcessorTransactionModelClass Object
	    vPTMCObj=[PaymentObjecthandler getModelObject];  //method to initialize the modal class object
	    vPTMCObj.transactionName = [self.transactionTypebtn titleForState:UIControlStateNormal];
	    vPTMCObj.state = [self.stateBtn titleForState:UIControlStateNormal]; 
	    vPTMCObj.country = self.countryTxtField.text; 
	    vPTMCObj.cardType = [self.cardTypeBtn titleForState:UIControlStateNormal]; 
	    vPTMCObj.cardholderName = self.nameTxtField.text; 
	    vPTMCObj.panNumber=self.creditCardNotxtField.text; 
	    vPTMCObj.expiryDate = [self.monthTextField.text stringByAppendingString:self.yearTextField.text]; 
	    vPTMCObj.street = self.streetTxtField.text; 
	    vPTMCObj.city = self.cityTxtField.text; 
	    vPTMCObj.stateProvince = [self.stateBtn titleForState:UIControlStateNormal]; 
	    vPTMCObj.accountType=@"NotSet";
	    vPTMCObj.postalCode = self.zipTxtField.text; 
	    vPTMCObj.phone= self.phoneTxtField.text; 
	    vPTMCObj.cvDataProvided = @"Provided";
	    vPTMCObj.cVData = self.cVCtxtField.text; 
	    vPTMCObj.amount = self.testCashTxtField.text; 
	    vPTMCObj.currencyCode = self.currencyCodeTxtField.text; 
	    vPTMCObj.customerPresent = @"Present";
	    vPTMCObj.employeeId = self.customerIDtxtField.text; 
	    vPTMCObj.entryMode = @"Keyed";
	    vPTMCObj.industryType = @"Ecommerce";
	    vPTMCObj.email = self.emailTxtField.text; 
	    vPTMCObj.countryCode = @"USA";
	    vPTMCObj.transactionDateTime=@"2013-04-03T13:50:16";
	    [PaymentObjecthandler setModelObject:vPTMCObj]; //method which sets value into modal class<br/>


1.Request a [velocityProcessorObj createCardToken];method from API .<br/> 
initialize response view class to store values in its variable<br/>
make property of VelocityResponase class <br/>

	@property (strong, nonatomic) VelocityResponse *_txRespons_obj;<br/>
initialize the object<br/>

 	self._txRespons_obj = [VelocityResponseObjectHandlers getModelObject];<br/>
 	
2.Get the success or Error response from API.<br/> 
       
	2.1 <b>-(void)VelocityProcessorFailedWithErrorMessage:(id )failedAny;</b><br/>
      
	 //Here get the Error status then show the corresponding message.	
          if (__txRespons_obj!=nil && [self._txRespons_obj isKindOfClass:[ErrorPaymentResponse 		  
          class]]&&__txRespons_obj.errorId!=nil) { 
						   }
	2.2 <b>-(void)VelocityProcessorFinishedWithSuccess:(id )successAny;</b><br/>

 	//Here get the Success status then show the corresponding message.
	if (__txRespons_obj!=nil && [self._txRespons_obj isKindOfClass:[BankcardTransactionResponsePro class]]&& 	
	__txRespons_obj.status!=nil ) { 
	//initialise VelocityPaymentTransactionModalClass object
	VelocityPaymentTransaction *obj =[PaymentObjecthandler getModelObject];
	//set value of payment account data token for transaction with token methos
	 obj.paymentAccountDataToken = self._txRespons_obj.paymentAccountDataToken;
					}

<h2>1.2 authorise(...);</h2><br/>
The method is responsible for the invocation of authorise operation on the Velocity REST server.<br/>

	<h2>1.2.1 authoriseWithToken();</h2><br/>
	<b>[velocityProcessorObj authorise];</b><br/>
	The method is responsible for the invocation of authorise operation with token on the Velocity REST server.<br/>
	for this you have to make pass the payment account data token to the library 

	<h2>1.2.2 authoriseWithoutToken();</h2><br/>
	<b>[velocityProcessorObj authorise];</b><br/>
	The method is responsible for the invocation of authorise operation without token on the Velocity REST server. this will be called only when payment account data token is null and card data should be present.<br/>
	<h2>1.2.3 authoriseP2PE();</h2><br/>
	The method is responsible for the invocation of authorise operation without token and with encrypted card data on the Velocity REST server. this will be called only when payment account data token is null and card data should be present.<br/>

Values are set from modal class <b>velocityPaymentTransaction</b> - holds the values for the authorize request VelocityPaymentTransaction<br/>
            	1.cardType - String     <br/> 
                2.cardholderName - String     <br/>
                3.panNumber-String   <br/>  			//require for without token operation
                4.expiryDate - String   <br/>  			//require for without token operation
	        5.street - String   <br/>
                6.stateProvince - String     <br/>
                7.postalCode - String   <br/>  
                8.phone - String    <br/>
	        9.state - String     <br/>
               10.cvDataProvided - String    <br/>
               11.cVData - String   <br/>  			//require for without token operation
	       12.reportingDataReference String <br/>
               13.transactionDataReference  String<br/>
	        14.amount - String       <br/>
               15.currencyCode - String       <br/> 
               16.customerPresent - String     <br/>
               17. partialShipment - boolean   <br/>
               18.signatureCaptured - boolean    <br/>
	       19.quasiCash - boolean    <br/>
               20.email - String   <br/>
               21.transactionDateTime - String   <br/>
               22.city -String <br/>
               23.partialApprovalCapable-String <br/>
               24.country - String     <br/>
               25.tipAmount - String   <br/>
               26.employeeId - String     <br/>
               27.entryMode - String      <br/>
	       28.industryType - String   <br/>
               29.countryCode - String     <br/>
               30.businnessName - String   <br/>
               31.comment - String    <br/>
               32.description - String    <br/>
               33.paymentAccountDataToken - String   <br/> 	//required for with token operation
               34.cashBackAmount - String       <br/> 
               35.goodsType - String     <br/>
               36.invoiceNumber - String     <br/>
               37.orderNumber - String      <br/>
	       38.FeeAmount - String   <br/>
	       39.securePaymentAccountData - String   <br/> 	//require for P2PE operation
	       40.encryptionKeyId - String   <br/> 		//require for P2PE operation
	       41.swipeStatus - String   <br/> 			//require for P2PE operation .if not available make it null.

                
	
<h2>How to set the Ui value on VelocityPaymentTransaction model</h2><br/>
<b>Sample code</b><br/> 
    
    VelocityPaymentTransaction *vPTMCObj;		 //velocityProcessorTransactionModelClass Object
    vPTMCObj=[PaymentObjecthandler getModelObject];	//method to initialize the modal class object
    vPTMCObj.transactionName = [self.transactionTypebtn titleForState:UIControlStateNormal];
    vPTMCObj.state = [self.stateBtn titleForState:UIControlStateNormal]; 
    vPTMCObj.country = self.countryTxtField.text; 
    vPTMCObj.amountforadjust = self.testCashAdjustTxtFeild.text; 
    vPTMCObj.cardType = [self.cardTypeBtn titleForState:UIControlStateNormal]; 
    vPTMCObj.cardholderName = self.nameTxtField.text; 
    vPTMCObj.panNumber=self.creditCardNotxtField.text; 
    vPTMCObj.expiryDate = [self.monthTextField.text stringByAppendingString:self.yearTextField.text]; 
    vPTMCObj.isNullable = false; 
    vPTMCObj.street = self.streetTxtField.text; 
    vPTMCObj.city = self.cityTxtField.text; 
    vPTMCObj.stateProvince = [self.stateBtn titleForState:UIControlStateNormal];
    vPTMCObj.accountType=@"NotSet";
    vPTMCObj.postalCode = self.zipTxtField.text;
    vPTMCObj.phone= self.phoneTxtField.text;
    vPTMCObj.cvDataProvided = @"Provided";
    vPTMCObj.cVData = self.cVCtxtField.text; 
    vPTMCObj.amount = self.testCashTxtField.text; 
    vPTMCObj.currencyCode = self.currencyCodeTxtField.text; 
    vPTMCObj.customerPresent = @"Present";
    vPTMCObj.employeeId = self.customerIDtxtField.text;
    vPTMCObj.entryMode = @"Keyed";
    vPTMCObj.industryType = @"Ecommerce";
    vPTMCObj.email = self.emailTxtField.text;
    vPTMCObj.countryCode = @"USA";
    vPTMCObj.businnessName = @"MomCorp";
    vPTMCObj.CustomerId = @"11";
    vPTMCObj.comment = @"a test comment";
    vPTMCObj.discription = @"a test description";
    vPTMCObj.reportingDataReference = @"001";
    vPTMCObj.transactionDataReference = @"xyt";
    vPTMCObj.transactionDateTime=@"2013-04-03T13:50:16";
    vPTMCObj.cashBackAmount = @"0.0";
    vPTMCObj.goodsType = @"NotSet";
    vPTMCObj.invoiceNumber = @"808";
    vPTMCObj.orderNumber = @"629203";
    vPTMCObj.FeeAmount = @"1000.05";
    vPTMCObj.tipAmount = self.tipAmountTxtField.text;	//this amount is used for capture<br/>
    vPTMCObj.keySerialNumber=@"";
    vPTMCObj.securePaymentAccountData = _securePaymentAccountDataTextView.text;//for P2PE only, leave null string in case not P2PE
    vPTMCObj.encryptionKeyId = _encryptionKeyIDTextView.text; //for P2PE only, leave null string in case not P2PE
    vPTMCObj.swipeStatus = @""; //for P2PE only, leave null string in case not P2PE
    vPTMCObj.isPartialShipment = false;
    vPTMCObj.isSignatureCaptured = false; 
    vPTMCObj.partialApprovalCapable = @"NotSet";
    vPTMCObj.isQuasiCash=false; 
    [PaymentObjecthandler setModelObject:vPTMCObj]; 
    
    
1.Request a authorize() method from API .<br/> 

		[velocityProcessorObj authorise];  	//calling authwithtoken method
		----------------------------------
		vPTMCObj.paymentAccountDataToken = nil;  //calling authwithout token method
            	[velocityProcessorObj authorise];	
        	------------------------------------
        	vPTMCObj.paymentAccountDataToken = nil;  //calling P2PE  method
        	vPTMCObj.encryptionKeyId = _encryptionKeyIDTextView.text; //for P2PE only, leave null string in case not P2PE
		vPTMCObj.swipeStatus = @""; //for P2PE only, leave null string in case not P2PE
    		vPTMCObj.securePaymentAccountData = _securePaymentAccountDataTextView.text;	//for P2PE only, leave null string in case not P2PE
            	[velocityProcessorObj authorise];
		-----------------------------------
	
if calling with token then have to set value for vPTMCobj.paymentaccountdDatatoken.<br/>
if calling without token   fill the card data information.
if calling P2PE securityaccount data token and encryption key id should be there.

2.Get the success or Error response from API.<br/> 
       
2.1 <b>-(void)VelocityProcessorFailedWithErrorMessage:(id )failedAny;</b><br/>
      
	 //Here get the Error status then show the corresponding message.	
          if (__txRespons_obj!=nil && [self._txRespons_obj isKindOfClass:[ErrorPaymentResponse 		  
          class]]&&__txRespons_obj.errorId!=nil) { 
						   }
2.2 <b>-(void)VelocityProcessorFinishedWithSuccess:(id )successAny; </b><br/>

 	//Here get the Success status then show the corresponding message.
	if (__txRespons_obj!=nil && [self._txRespons_obj isKindOfClass:[BankcardTransactionResponsePro class]]&& 	
	__txRespons_obj.status!=nil ) { 
	//Assign value for further transection data 
	  
	 VelocityPaymentTransaction *obj =[PaymentObjecthandler getModelObject];<br/>
		obj.transectionID=self._txRespons_obj.transactionId;   //set value for TransectionID<br/>
		obj.paymentAccountDataToken = self._txRespons_obj.paymentAccountDataToken; //set value for payentAccount data token<br/>
		obj.batchID =self._txRespons_obj.batchId; //set value for batchID<br/>
	 	}

<h2>1.3 authAndCapture(...) </h2><br/>
The method is responsible for the invocation of authorizeAndCapture operation on the Velocity REST server.<br/>
 <b> -(void)authorizeAndCapture;</b><br/>

Set paramets in the modal class <b>velocityPaymentTransaction </b><br/>
	        1. cardType - String     <br/>
		2. cardholderName - String     <br/>
	        3.panNumber-String   <br/>
	        4.expiryDate - String   <br/>
		5.street - String   <br/>
	        6. stateProvince - String     <br/>
	        7.postalCode - String   <br/>
	        8. phone - String    <br/>
		9. reportingDataReference String <br/>
	        10. transactionDataReference  String<br/>
		11. state - String     <br/>
	        12. cvDataProvided - String    <br/>
	        13.cVData - String   <br/>
		14. amount - String       <br/>
	        15. currencyCode - String       <br/> 
	        16. customerPresent - String     <br/>
	        17. employeeId - String     <br/>
	        18. entryMode - String      <br/>
		19.industryType - String   <br/>
	        20. email - String   <br/>
		21. transactionDateTime - String   <br/>
		22. city -String <br/>
		23. partialShipment - boolean   <br/>
	        24.country - String     <br/>
	        25.signatureCaptured - boolean    <br/>
	        26.tipAmount - String   <br/>
	        27. quasiCash - boolean    <br/>
		28.partialApprovalCapable - String   <br/>
	        29. countryCode - String     <br/>
	        30. businnessName - String   <br/>
	        31.comment - String    <br/>
	        32.description - String    <br/>
	        33.paymentAccountDataToken - String   <br/>
	        34.cashBackAmount - String       <br/> 
	        35.goodsType - String     <br/>
	        36.invoiceNumber - String     <br/>
	        37.orderNumber - String      <br/>
		38.FeeAmount - String   <br/>
             
 <h2>How to set the Ui value on VelocityPaymentTransaction model </h2><br/>
 <b>Sample code</b><br/> 
 
  	VelocityPaymentTransaction *vPTMCObj;//velocityProcessorTransactionModelClass Object
    	vPTMCObj=[PaymentObjecthandler getModelObject];//method to initialize the modal class object
    	vPTMCObj.transactionName = [self.transactionTypebtn titleForState:UIControlStateNormal];
    	vPTMCObj.state = [self.stateBtn titleForState:UIControlStateNormal]; 
    	vPTMCObj.country = self.countryTxtField.text; 
    	vPTMCObj.amountforadjust = self.testCashAdjustTxtFeild.text; 
    	vPTMCObj.cardType = [self.cardTypeBtn titleForState:UIControlStateNormal]; 
    	vPTMCObj.cardholderName = self.nameTxtField.text; 
    	vPTMCObj.panNumber=self.creditCardNotxtField.text; 
	vPTMCObj.expiryDate = [self.monthTextField.text stringByAppendingString:self.yearTextField.text]; 
	vPTMCObj.isNullable = false; 
    	vPTMCObj.street = self.streetTxtField.text; 
    	vPTMCObj.city = self.cityTxtField.text; 
    	vPTMCObj.stateProvince = [self.stateBtn titleForState:UIControlStateNormal];
    	vPTMCObj.accountType=@"NotSet";
    	vPTMCObj.postalCode = self.zipTxtField.text;
    	vPTMCObj.phone= self.phoneTxtField.text;
    	vPTMCObj.cvDataProvided = @"Provided";
    	vPTMCObj.cVData = self.cVCtxtField.text; 
    	vPTMCObj.amount = self.testCashTxtField.text; 
    	vPTMCObj.currencyCode = self.currencyCodeTxtField.text; 
    	vPTMCObj.customerPresent = @"Present";
    	vPTMCObj.employeeId = self.customerIDtxtField.text;
    	vPTMCObj.entryMode = @"Keyed";
    	vPTMCObj.industryType = @"Ecommerce";
    	vPTMCObj.email = self.emailTxtField.text;
    	vPTMCObj.countryCode = @"USA";
    	vPTMCObj.businnessName = @"MomCorp";
    	vPTMCObj.CustomerId = @"11";
    	vPTMCObj.comment = @"a test comment";
    	vPTMCObj.discription = @"a test description";
    	vPTMCObj.reportingDataReference = @"001";
    	vPTMCObj.transactionDataReference = @"xyt";
    	vPTMCObj.transactionDateTime=@"2013-04-03T13:50:16";
    	vPTMCObj.cashBackAmount = @"0.0";
    	vPTMCObj.goodsType = @"NotSet";
    	vPTMCObj.invoiceNumber = @"808";
    	vPTMCObj.orderNumber = @"629203";
    	vPTMCObj.FeeAmount = @"1000.05";
    	vPTMCObj.tipAmount = self.tipAmountTxtField.text;//this amount is used for capture<br/>
    	vPTMCObj.securePaymentAccountData = @"";
    	vPTMCObj.encryptionKeyId = @"";
    	vPTMCObj.swipeStatus = @"";
    	vPTMCObj.isPartialShipment = false;
    	vPTMCObj.isSignatureCaptured = false; 
    	vPTMCObj.partialApprovalCapable = @"NotSet";
      	vPTMCObj.isQuasiCash=false; 
    	[PaymentObjecthandler setModelObject:vPTMCObj]; 

     
1.Request a AuthANdCapture() method from API .<br/>

	[velocityProcessorObj authorizeAndCapture];   //calling with token 
	------------------------------------
	vPTMCObj.paymentAccountDataToken = nil;  //calling authwithout token method
	[velocityProcessorObj authorizeAndCapture];    //calling without token
	-------------------------------------
	vPTMCObj.paymentAccountDataToken = nil;  //calling P2PE  method
        	vPTMCObj.encryptionKeyId = _encryptionKeyIDTextView.text; //for P2PE only, leave null string in case not P2PE
		vPTMCObj.swipeStatus = @""; //for P2PE only, leave null string in case not P2PE
    		vPTMCObj.securePaymentAccountData = _securePaymentAccountDataTextView.text;	//for P2PE only, leave null string in case not P2PE
	[velocityProcessorObj authorizeAndCapture];    //For P2PE
	
if calling with token then have to set value for vPTMCobj.paymentaccountdDatatoken.<br/>
Other wise have to fill the card data information for without token and secureaccount data token ,encryption key id for P2PE
2.Get the success or Error response from API.<br/> 
       
2.1 <b>-(void)VelocityProcessorFailedWithErrorMessage:(id )failedAny;</b><br/>
      
	 //Here get the Error status then show the corresponding message.	
          if (__txRespons_obj!=nil && [self._txRespons_obj isKindOfClass:[ErrorPaymentResponse 		  
          class]]&&__txRespons_obj.errorId!=nil) { 
						   }
2.2 <b>-(void)VelocityProcessorFinishedWithSuccess:(id )successAny; </b><br/>

 	//Here get the Success status then show the corresponding message.
	        if (__txRespons_obj!=nil&&[self._txRespons_objisKindOfClass:[BancardCaptureResponseclass]]&&__txRespons_obj.status!  =nil){
	//Assign value for further transection data 
	  
	 VelocityPaymentTransaction *obj =[PaymentObjecthandler getModelObject];<br/>
		obj.transectionID=self._txRespons_obj.transactionId;   //set value for TransectionID<br/>
		obj.paymentAccountDataToken = self._txRespons_obj.paymentAccountDataToken; //set value for payentAccount data token<br/>
		obj.batchID =self._txRespons_obj.batchId; //set value for batchID<br/>
	 
	
					}

<h2>1.4 capture(...) </h2><br/>
The method is responsible for the invocation of capture operation on the Velocity REST server.<br/>
<b> [velocityProcessorObj capture];</b><br/>


  <h2>How to set the Ui value on VelocityPaymentTransaction model </h2><br/>
  <b>Sample code</b><br/> 
  
 	    VelocityPaymentTransaction *vPTMCObj;//velocityProcessorTransactionModelClass Object
    	vPTMCObj=[PaymentObjecthandler getModelObject];//method to initialize the modal class object
    	vPTMCObj.amount = self.testCashTxtField.text; 
    	vPTMCObj.tipAmount = self.tipAmountTxtField.text;//this amount is used for capture<br/>
    	//transection id should be set in the sucess response of previous authorised method.
    	[PaymentObjecthandler setModelObject:vPTMCObj]; 


       1.Request a capture() method from API .<br/> 
       [velocityProcessorObj capture];<br/>
       2.Get the success or Error response from API.<br/> 
       
	2.1 <b>-(void)VelocityProcessorFailedWithErrorMessage:(id )failedAny;</b><br/>
      
	 //Here get the Error status then show the corresponding message.	
          if (__txRespons_obj!=nil && [self._txRespons_obj isKindOfClass:[ErrorPaymentResponse 		  
          class]]&&__txRespons_obj.errorId!=nil) { 
						   }
	2.2 <b>-(void)VelocityProcessorFinishedWithSuccess:(id )successAny; </b><br/>
 	//Here get the Success status then show the corresponding message.
	if (__txRespons_obj!=nil && [self._txRespons_obj isKindOfClass:[BancardCaptureResponse 				  
		class]]&&__txRespons_obj.status!=nil){
	 VelocityPaymentTransaction *obj =[PaymentObjecthandler getModelObject];<br/>
		obj.transectionID=self._txRespons_obj.transactionId;   //set value for TransectionID<br/>//save transection 				id       for void method,return by id and return unlinked method
					}
<h2>1.5 undo(...) </h2><br/>
The method is responsible for the invocation of undo operation on the Velocity REST server.<br/>
<b> -(void)undo; </b><br/>


<h2>How to set the Ui value on VelocityPaymentTransaction model </h2><br/>
  Value for trANSECTION ID is assigned from the privious sucess delegate method of velocity processor delegate
  
   <b>Sample code</b><br/> 
       1.Request a undo() method from API .<br/> 
       [velocityProcessorObj undo];
       set the transection id 
       2.Get the success or Error response from API.<br/> 
       
	2.1 <b>-(void)VelocityProcessorFailedWithErrorMessage:(id )failedAny;</b><br/>
      
	 //Here get the Error status then show the corresponding message.	
          if (__txRespons_obj!=nil && [self._txRespons_obj isKindOfClass:[ErrorPaymentResponse 		  
          class]]&&__txRespons_obj.errorId!=nil) { 
						   }
	2.2 <b>-(void)VelocityProcessorFinishedWithSuccess:(id )successAny; </b><br/>
 	//Here get the Success status then show the corresponding message.
	if (__txRespons_obj!=nil && [self._txRespons_obj isKindOfClass:[BankcardTransactionResponsePro class]]&& 	
	__txRespons_obj.status!=nil ) { 
	 VelocityPaymentTransaction *obj =[PaymentObjecthandler getModelObject];<br/>
		obj.transectionID=self._txRespons_obj.transactionId;   //set value for TransectionID<br/>//save transection 				id       for void method,return by id and return unlinked method
					}

<h2>1.6 adjust(...) </h2><br/>
The method is responsible for the invocation of adjust operation on the Velocity REST server.<br/>
<b> -(void)adjust;</b><br/>

                  1.amountfordjust - String     <br/>
		  2.transactionId - String      <br/>
		  
<h2>How to set the Ui value on VelocityPaymentTransaction model </h2><br/>
 <b>Sample code</b><br/> 
 
		    VelocityPaymentTransaction *vPTMCObj;//velocityProcessorTransactionModelClass Object<br/>
    		vPTMCObj=[PaymentObjecthandler getModelObject];//method to initialize the modal class object<br/>
		    vPTMCObj.amountforadjust = self.testCashAdjustTxtFeild.text;<br/>
		
 	1.Request a adjust() method from API .<br/> 
       		[velocityProcessorObj adjust];<br/>
       2.Get the success or Error response from API.<br/> 
       
	2.1 <b>-(void)VelocityProcessorFailedWithErrorMessage:(id )failedAny;</b><br/>
      
	 //Here get the Error status then show the corresponding message.	
          if (__txRespons_obj!=nil && [self._txRespons_obj isKindOfClass:[ErrorPaymentResponse 		  
          class]]&&__txRespons_obj.errorId!=nil) { 
						   }
	2.2 <b>-(void)VelocityProcessorFinishedWithSuccess:(id )successAny; </b><br/>
 	//Here get the Success status then show the corresponding message.
	if (__txRespons_obj!=nil && [self._txRespons_obj isKindOfClass:[BankcardTransactionResponsePro class]]&& 	
	__txRespons_obj.status!=nil ) { 
	 VelocityPaymentTransaction *obj =[PaymentObjecthandler getModelObject];<br/>
		obj.transectionID=self._txRespons_obj.transactionId;   //set value for TransectionID<br/>//save transection 				id       for void method,return by id and return unlinked method
					}

<h2>1.7 returnById(...) </h2><br/>
The method is responsible for the invocation of returnById operation on the Velocity REST server.<br/>
<b>-(void)returnById;</b><br/>

Modal class  <b>velocityPaymentTransaction </b> - holds the values for the returnById request VelocityPaymentTransaction <br/>
                  1. transactionId - String <br/>
		              2. amount - String   <br/> 
<h2>How to set the Ui value on VelocityPaymentTransaction model </h2><br/>
<b>Sample code</b><br/> 
	    VelocityPaymentTransaction *vPTMCObj;//velocityProcessorTransactionModelClass Object
    	vPTMCObj=[PaymentObjecthandler getModelObject];//method to initialize the modal class object
	    vPTMCObj.amount = self.testCashTxtField.text;
	    //take transection id prom previous capture method success delegate

       1.Request a returnById() method from API .<br/> 
        [velocityProcessorObj returnById];<br/>
       2.Get the success or Error response from API.<br/> 
       
	2.1 <b>-(void)VelocityProcessorFailedWithErrorMessage:(id )failedAny;</b><br/>
      
	 //Here get the Error status then show the corresponding message.	
          if (__txRespons_obj!=nil && [self._txRespons_obj isKindOfClass:[ErrorPaymentResponse 		  
          class]]&&__txRespons_obj.errorId!=nil) { 
						   }
	2.2 <b>-(void)VelocityProcessorFinishedWithSuccess:(id )successAny; </b><br/>
 	//Here get the Success status then show the corresponding message.
	if (__txRespons_obj!=nil && [self._txRespons_obj isKindOfClass:[BankcardTransactionResponsePro class]]&& 	
	__txRespons_obj.status!=nil ) { 
	 VelocityPaymentTransaction *obj =[PaymentObjecthandler getModelObject];<br/>
		obj.transectionID=self._txRespons_obj.transactionId;   //set value for TransectionID<br/>//save transection 				id for void method,return by id and return unlinked method
					}

<h2>1.8 returnUnLinked(...) </h2><br/>
The method is responsible for the invocation of returnUnLinked operation on the Velocity REST server.<br/>
<b> -(void)returnUnlinked;</b><br/>

Modal class  <b>velocityPaymentTransaction </b> - holds the values for the returnUnlinked request VelocityPaymentTransaction<br/>
	     1. cardType - String     <br/>
		2. cardholderName - String     <br/>
	     3.  panNumber-String   <br/>
	     4.   expiryDate - String   <br/>
		      	 5.   street - String   <br/>
	     6.   stateProvince - String     <br/>
	     7.   postalCode - String   <br/>
	     8.   phone - String    <br/>
			       9.    reportingDataReference String <br/>
	    10.   transactionDataReference  String<br/>
			      11.   state - String     <br/>
	    12.   cvDataProvided - String    <br/>
			      13.   amount - String       <br/>
	    14.   currencyCode - String       <br/> 
	    15.   customerPresent - String     <br/>
	    16.   employeeId - String     <br/>
	    17.   entryMode - String      <br/>
	          18.   industryType - String   <br/>
	    19.   email - String   <br/>
			      20.   transactionDateTime - String   <br/>
			      21.   city -String <br/>
			      22.   quasiCash - boolean    <br/>
	    23.  country - String     <br/>
	    24. transactionId- String <br/>
	    25.  signatureCaptured - boolean    <br/>
	    26.  partialShipment - boolean   <br/>
	    27.  countryCode - String     <br/>
	    28.  businnessName - String   <br/>
	    29. comment - String    <br/>
	    30. description - String    <br/>
	    31. paymentAccountDataToken - String   <br/>
	    32. cashBackAmount - String       <br/> 
	    33. goodsType - String     <br/>
	    34.invoiceNumber - String     <br/>
	    35.orderNumber - String      <br/>
	          36. FeeAmount - String   <br/>
	    37. tipAmount - String   <br/>
	    38. partialApprovalCapable - String   <br/>
            
<h2>How to set the Ui value on VelocityPaymentTransaction model </h2><br/>

 
<b>Sample code</b><br/> 

	    VelocityPaymentTransaction *vPTMCObj;//velocityProcessorTransactionModelClass Object
    	vPTMCObj=[PaymentObjecthandler getModelObject];//method to initialize the modal class object
    	vPTMCObj.transactionName = [self.transactionTypebtn titleForState:UIControlStateNormal];
    	vPTMCObj.state = [self.stateBtn titleForState:UIControlStateNormal]; 
    	vPTMCObj.country = self.countryTxtField.text; 
    	vPTMCObj.amountforadjust = self.testCashAdjustTxtFeild.text; 
    	vPTMCObj.cardType = [self.cardTypeBtn titleForState:UIControlStateNormal]; 
    	vPTMCObj.cardholderName = self.nameTxtField.text; 
    	vPTMCObj.panNumber=self.creditCardNotxtField.text; 
    	vPTMCObj.expiryDate = [self.monthTextField.text stringByAppendingString:self.yearTextField.text]; 
    	vPTMCObj.isNullable = false; 
    	vPTMCObj.street = self.streetTxtField.text; 
    	vPTMCObj.city = self.cityTxtField.text; 
    	vPTMCObj.stateProvince = [self.stateBtn titleForState:UIControlStateNormal];
    	vPTMCObj.accountType=@"NotSet";
	    vPTMCObj.postalCode = self.zipTxtField.text;
    	vPTMCObj.phone= self.phoneTxtField.text;
    	vPTMCObj.cvDataProvided = @"Provided";
    	vPTMCObj.cVData = self.cVCtxtField.text; 
    	vPTMCObj.amount = self.testCashTxtField.text; 
	    vPTMCObj.currencyCode = self.currencyCodeTxtField.text; 
    	vPTMCObj.customerPresent = @"Present";
    	vPTMCObj.employeeId = self.customerIDtxtField.text;
    	vPTMCObj.entryMode = @"Keyed";
    	vPTMCObj.industryType = @"Ecommerce";
    	vPTMCObj.email = self.emailTxtField.text;
    	vPTMCObj.countryCode = @"USA";
    	vPTMCObj.businnessName = @"MomCorp";
    	vPTMCObj.CustomerId = @"11";
    	vPTMCObj.comment = @"a test comment";
	    vPTMCObj.discription = @"a test description";
    	vPTMCObj.reportingDataReference = @"001";
    	vPTMCObj.transactionDataReference = @"xyt";
    	vPTMCObj.transactionDateTime=@"2013-04-03T13:50:16";
    	vPTMCObj.cashBackAmount = @"0.0";
    	vPTMCObj.goodsType = @"NotSet";
    	vPTMCObj.invoiceNumber = @"808";
    	vPTMCObj.orderNumber = @"629203";
    	vPTMCObj.FeeAmount = @"1000.05";
    	vPTMCObj.tipAmount = self.tipAmountTxtField.text;//this amount is used for capture<br/>
    	vPTMCObj.securePaymentAccountData = @""; //ONLY FOR P2PE
    	vPTMCObj.encryptionKeyId = @"";		//ONLY FOR P2PE
    	vPTMCObj.swipeStatus = @"";
      	vPTMCObj.isPartialShipment = false;
    	vPTMCObj.isSignatureCaptured = false; 
    	vPTMCObj.partialApprovalCapable = @"NotSet";
       	vPTMCObj.isQuasiCash=false; 
    	[PaymentObjecthandler setModelObject:vPTMCObj]; 
    	
       1.Request a returnUnLinked() method from API .<br/> 
       
		[velocityProcessorObj returnUnlinked]; //return unlinked method call
		--------------------------------------
		vPTMCObj.paymentAccountDataToken = nil;  //calling authwithout token method
		[velocityProcessorObj returnUnlinked];  //return unlinked without token method call
		---------------------------------------
		vPTMCObj.paymentAccountDataToken = nil;  //calling P2PE  method
        	vPTMCObj.encryptionKeyId = _encryptionKeyIDTextView.text; //for P2PE only, leave null string in case not P2PE
		vPTMCObj.swipeStatus = @""; //for P2PE only, leave null string in case not P2PE
    		vPTMCObj.securePaymentAccountData = _securePaymentAccountDataTextView.text;	//for P2PE only, leave null string in case not P2PE
		[velocityProcessorObj returnUnlinked];  //For P2PE
	2.Get the success or Error response from API.<br/> 
       
	2.1 <b>-(void)VelocityProcessorFailedWithErrorMessage:(id )failedAny;</b><br/>
      
	 //Here get the Error status then show the corresponding message.	
          if (__txRespons_obj!=nil && [self._txRespons_obj isKindOfClass:[ErrorPaymentResponse 		  
          class]]&&__txRespons_obj.errorId!=nil) { 
						   }
	2.2 <b>-(void)VelocityProcessorFinishedWithSuccess:(id )successAny; </b><br/>
 	//Here get the Success status then show the corresponding message.
	      if (__txRespons_obj!=nil && [self._txRespons_obj isKindOfClass:[BankcardTransactionResponsePro class]]&& 	
    	__txRespons_obj.status!=nil ) { 
	       VelocityPaymentTransaction *obj =[PaymentObjecthandler getModelObject];<br/>
	    	obj.transectionID=self._txRespons_obj.transactionId;   //set value for TransectionID<br/>//save transection 				id for void method,return by id and return unlinked method
					}
<h2>1.9 QueryTransactionDetail()</h2><br/>
The method is responsible for the invocation of queryTransaction operation on the Velocity REST server.<br/>

<b> -(void)queryTransactionsDetail;</b><br/>

Modal class  <b>velocityPaymentTransaction </b> - holds the values for the returnUnlinked request VelocityPaymentTransaction<br/>
	     1. cardType - String     <br/>
		2. cardholderName - String     <br/>
	     3.  panNumber-String   <br/>
	     4.   expiryDate - String   <br/>
		      	 5.   street - String   <br/>
	     6.   stateProvince - String     <br/>
	     7.   postalCode - String   <br/>
	     8.   phone - String    <br/>
			       9.    reportingDataReference String <br/>
	    10.   transactionDataReference  String<br/>
			      11.   state - String     <br/>
	    12.   cvDataProvided - String    <br/>
			      13.   amount - String       <br/>
	    14.   currencyCode - String       <br/> 
	    15.   customerPresent - String     <br/>
	    16.   employeeId - String     <br/>
	    17.   entryMode - String      <br/>
	          18.   industryType - String   <br/>
	    19.   email - String   <br/>
			      20.   transactionDateTime - String   <br/>
			      21.   city -String <br/>
			      22.   quasiCash - boolean    <br/>
	    23.  country - String     <br/>
	    24. transactionId- String <br/>
	    25.  signatureCaptured - boolean    <br/>
	    26.  partialShipment - boolean   <br/>
	    27.  countryCode - String     <br/>
	    28.  businnessName - String   <br/>
	    29. comment - String    <br/>
	    30. description - String    <br/>
	    31. paymentAccountDataToken - String   <br/>
	    32. cashBackAmount - String       <br/> 
	    33. goodsType - String     <br/>
	    34.invoiceNumber - String     <br/>
	    35.orderNumber - String      <br/>
	          36. FeeAmount - String   <br/>
	    37. tipAmount - String   <br/>
	    38. partialApprovalCapable - String   <br/>
            
<h2>How to set the Ui value on VelocityPaymentTransaction model </h2><br/>

 
<b>Sample code</b><br/> 

	    VelocityPaymentTransaction *vPTMCObj;//velocityProcessorTransactionModelClass Object
    	vPTMCObj=[PaymentObjecthandler getModelObject];//method to initialize the modal class object
    	vPTMCObj.transactionName = [self.transactionTypebtn titleForState:UIControlStateNormal];
    	vPTMCObj.state = [self.stateBtn titleForState:UIControlStateNormal]; 
    	vPTMCObj.country = self.countryTxtField.text; 
    	vPTMCObj.amountforadjust = self.testCashAdjustTxtFeild.text; 
    	vPTMCObj.cardType = [self.cardTypeBtn titleForState:UIControlStateNormal]; 
    	vPTMCObj.cardholderName = self.nameTxtField.text; 
    	vPTMCObj.panNumber=self.creditCardNotxtField.text; 
    	vPTMCObj.expiryDate = [self.monthTextField.text stringByAppendingString:self.yearTextField.text]; 
    	vPTMCObj.isNullable = false; 
    	vPTMCObj.street = self.streetTxtField.text; 
    	vPTMCObj.city = self.cityTxtField.text; 
    	vPTMCObj.stateProvince = [self.stateBtn titleForState:UIControlStateNormal];
    	vPTMCObj.accountType=@"NotSet";
	    vPTMCObj.postalCode = self.zipTxtField.text;
    	vPTMCObj.phone= self.phoneTxtField.text;
    	vPTMCObj.cvDataProvided = @"Provided";
    	vPTMCObj.cVData = self.cVCtxtField.text; 
    	vPTMCObj.amount = self.testCashTxtField.text; 
	    vPTMCObj.currencyCode = self.currencyCodeTxtField.text; 
    	vPTMCObj.customerPresent = @"Present";
    	vPTMCObj.employeeId = self.customerIDtxtField.text;
    	vPTMCObj.entryMode = @"Keyed";
    	vPTMCObj.industryType = @"Ecommerce";
    	vPTMCObj.email = self.emailTxtField.text;
    	vPTMCObj.countryCode = @"USA";
    	vPTMCObj.businnessName = @"MomCorp";
    	vPTMCObj.CustomerId = @"11";
    	vPTMCObj.comment = @"a test comment";
	    vPTMCObj.discription = @"a test description";
    	vPTMCObj.reportingDataReference = @"001";
    	vPTMCObj.transactionDataReference = @"xyt";
    	vPTMCObj.transactionDateTime=@"2013-04-03T13:50:16";
    	vPTMCObj.cashBackAmount = @"0.0";
    	vPTMCObj.goodsType = @"NotSet";
    	vPTMCObj.invoiceNumber = @"808";
    	vPTMCObj.orderNumber = @"629203";
    	vPTMCObj.FeeAmount = @"1000.05";
    	vPTMCObj.tipAmount = self.tipAmountTxtField.text;//this amount is used for capture<br/>
    	vPTMCObj.securePaymentAccountData = @""; //ONLY FOR P2PE
    	vPTMCObj.encryptionKeyId = @"";		//ONLY FOR P2PE
    	vPTMCObj.swipeStatus = @"";
      	vPTMCObj.isPartialShipment = false;
    	vPTMCObj.isSignatureCaptured = false; 
    	vPTMCObj.partialApprovalCapable = @"NotSet";
       	vPTMCObj.isQuasiCash=false; 
    	[PaymentObjecthandler setModelObject:vPTMCObj]; 
    	
       1.Request a queryTransactionDetail() method from API .<br/> 
       
		[velocityProcessorObj queryTransactionsDetail]; //return unlinked method call
		      queryTransactionRequestModalObject.page = @"0";
            queryTransactionRequestModalObject.pageSize = @"10";
            queryTransactionRequestModalObject.transactionStartDateTime = @"";
            queryTransactionRequestModalObject.transactionEndDateTime = @"";
            queryTransactionRequestModalObject.includeRelated = @"true";
            queryTransactionRequestModalObject.isAcknowledged = @"NotSet";
            queryTransactionRequestModalObject.transactionIds = @[_queryTransactionIDTextField.text];
            queryTransactionRequestModalObject.batchIds = @[_queryBatchID.text];
    	
	
	2.Get the success or Error response from API.<br/> 
       
	2.1 <b>-(void)VelocityProcessorFailedWithErrorMessage:(id )failedAny;</b><br/>
      
	2.2 <b>-(void)VelocityProcessorFinishedWithSuccess:(id )successAny; </b><br/>
 <h2>1.10 CaptureAll()</h2><br/>
The method is responsible for the invocation of CaptureAll on the Velocity REST server.<br/>

<b> -(void)captureAll;</b><br/>	
 2.Get the success or Error response from API.<br/> 
       
	2.1 <b>-(void)VelocityProcessorFailedWithErrorMessage:(id )failedAny;</b><br/>
	 
      
	2.2 <b>-(void)VelocityProcessorFinishedWithSuccess:(id )successAny; </b><br/>
	else if (__txRespons_obj!=nil && [self._txRespons_obj isKindOfClass:[CaptureAllArrayOfResponse class]]&&__txRespons_obj!=nil){
	 }
 
<h2>2. VelocityResponse </h2><br/>

This Modal class implements the responses coming from the Velocity server for a payment transaction request. <br/>
It has the following attributes with name and datatype is string.<br/>
		1.status;
		2.statusCode;
		3.statusMessage;
		4.transactionId;
		5.originatorTransactionId;
		6.serviceTransactionId;
		7.serviceTransactionDateTime;
		8.captureState;
		9.transactionState;
		10.acknowledged; //it is bool type
		11.prepaidCard;
		12.reference;
		13.amount;
		14.cardType;
		15.feeAmount;
		16.approvalCode;
		17.aVSResult;
		18.batchId;
		19.cVResult;
		20.cardLevel;
		21.downgradeCode;
		22.maskedPAN;
		23.paymentAccountDataToken ;
		24.retrievalReferenceNumber;
		25.adviceResponse;
		26.commercialCardResponse;
		27.returnedACI;
		28.statusHttpResponse;
		29.statusCodeHttpResponse;
		30.orderId;
		31.settlementDate;
		32.finalBalance;
		33.cashBackAmount;
		34.date;
		35.time;
		36.timeZone;
		37.errorId;
		38.helpUrl;
		39.operation;
		40.reason;
		41.ruleMessage;
		42.netAmount;
		43.count;
		44.httpCode;

 it has one method which provides user an ease to get data from the modal class
 	
 		+(VelocityResponse *)getModelObject;
 	
 sample code
 
 	@property (strong, nonatomic) VelocityResponse *_txRespons_obj;
	self._txRespons_obj = [VelocityResponseObjectHandlers getModelObject];

<h2>2.1 BankcardTransactionResponsePro</h2><br/>

This class has the following main attributes with its name and data type. <br/>
     1.   status - String     <br/>
     2.   statusCode - String     <br/>
     3.   statusMessage - String     <br/>
     4.   transactionId - String     <br/>
     5.   originatorTransactionId - String     <br/>
     6.   serviceTransactionId - String     <br/
     7.   addendum - Addendum    <br/>
     8.   captureState - String     <br/>
      9.  transactionState - String     <br/>
     10.  acknowledged - boolean   <br/>
     11.  prepaidCard - String     <br/>
     12.  reference - String     <br/>
     13.  amount - String     <br/>
     14.  cardType - String     <br/>
     15.  feeAmount - String    <br/>
     16.  approvalCode - String     <br/>
     17.  avsResult - AVSResult     <br/>
     18.  batchId - String    <br/>
     19.  cVResult - String   <br/>
     20.  cardLevel - String   <br/>
     21.  downgradeCode - String  <br/>
     22.  maskedPAN - String    <br/>
     23.  paymentAccountDataToken - String    <br/>
     24.  retrievalReferenceNumber - String      <br/>
     25.  resubmit - String    <br/>
     26.  settlementDate - String   <br/>
     27.  finalBalance - String     <br/>
     28.  orderId - String       <br/>
     29.  cashBackAmount - String    <br/>
     30.  adviceResponse - String     <br/>
	   31.   date - String   <br/>
     32.   time - String   <br/>
     33.  time zone - String     <br/>
     34.  commercialCardResponse -  String     <br/>
     35.  returnedACI - String       <br/><br/>
     
<h2>2.2 BankcardCaptureResponse </h2><br/>     
   
This class has the following main attributes with its name and datatype. <br/>     
     1.   status - String     <br/>
     2.   statusCode - String     <br/>
     3.   statusMessage - String     <br/>
     4.   transactionId - String     <br/>
     5.   originatorTransactionId - String     <br/>
     6.   serviceTransactionId - String     <br/>
     7.   date - String   <br/>
     8.   time - String   <br/>
     9.   time zone - String     <br/>
     10.  addendum - Addendum   <br/>
     11.  captureState - String    <br/>
     12.  transactionState - String    <br/>
     13.  acknowledged - String   <br/>
     14.  reference - String       <br/>
     15.  batchId - String       <br/> 
     16.  industryType - String     <br/>
     17.  transactionSummaryData - TransactionSummaryData     <br/>
     18.  prepaidCard - String      <br/>
     
 <h2>2.3 ErrorResponse </h2><br/>     
 
This class has the following main attributes with its name and data type. <br/>     
   1.  errorId - String   <br/>
   2.  helpUrl - String   <br/>
   3.  operation - String    <br/>
   4.  reason - String   <br/>
   5.  ruleMessage-String    <br/>
   6.  ruleKey-String <br/>
   7.  ruleLocationKey- String <br/>
   8.  transactionId - String <br/>
   
<h2>3.VelocityPaymentTransaction </h2><br/>
This class is mainly used to store the user interface value .<br/>
This class has the following main attributes with its name and data-type.<br/>
     1.   transactionName - String     <br/>
     2.   state - String     <br/>
     3.   country - String     <br/>
     4.   amountfordjust - String     <br/>
     5.   cardType - String     <br/>
     6.   cardholderName - String     <br/>
     7.   panNumber-String   <br/>
     8.   expiryDate - String   <br/>
     9.   street - String   <br/>
     10.  stateProvince - String     <br/>
     11.  postalCode - String   <br/>
     12.  phone - String    <br/>
     13.  cvDataProvided - String    <br/>
     14.  cVData - String   <br/>
     15.  amount - String       <br/>
     16.  currencyCode - String       <br/> 
     17.  customerPresent - String     <br/>
     18.  employeeId - String     <br/>
     19.  entryMode - String      <br/>
	   20.   industryType - String   <br/>
     21.   email - String   <br/>
     22.  countryCode - String     <br/>
     23.  businnessName - String   <br/>
     24.  comment - String    <br/>
     25.  description - String    <br/>
     26.  paymentAccountDataToken - String   <br/>
     27.  transactionDateTime - String       <br/>
     28.  cashBackAmount - String       <br/> 
     29.  goodsType - String     <br/>
     30.  invoiceNumber - String     <br/>
     31.  orderNumber - String      <br/>
	   32.   FeeAmount - String   <br/>
     33.   tipAmount - String   <br/>
     34.  accountType - String     <br/>
     35.  partialApprovalCapable - String   <br/>
     36.  quasiCash - boolean    <br/>
     37.  signatureCaptured - boolean    <br/>
     38.  partialShipment - boolean   <br/>
     39.  transactionId - String       <br/>
     40.  cashBackAmount - String       <br/> 
     41.  goodsType - String     <br/>
     42. reportingDataReference String <br/>
     43. transactionDataReference  String<br/>  
	   44. customerId - String      <br/>
	   45.  city - String <br/>

<h2>3.Velocity sample IOSApplication </h2><br/>
The velocity  sample IOSApplication is responsible for putting the Velocity IOS-SDK for test purpose. <br/>
It intends to perform the testing of transaction methods available on the Velocity payment gateway for a merchant. <br/>

When a transaction method needs to invoke from Velocity server then it sends the transaction request data and receives the response depending on the type of transaction performed on the velocity server.<br/>
The request data is send through the User Interface form which includes the fields required for a transaction. <br/>

The Velocity sample IOS Application is able to test the following transaction methods through its user interface. <br/>

1. Verify - The Verify operation is used to verify information about a payment account, such as address verification data (AVSData) on a credit card account, without reserving any funds. <br/>
2. Authorize - The Authorize operation is used to authorize transactions by performing a check on card-holder's funds and reserves the authorization amount if sufficient funds are available. <br/>
3. Authorize W/O token - This method proceeds with the card details when payment account data token is not available. <br/>
4.P2PE Authorize - This method proceeds with the encrypted card details when payment account data token is not available. <br/> 
5. AuthorizeAndCapture - The AuthorizeAndCapture operation is used to authorize transactions by performing a check on card-holder's funds and reserves the authorization amount if sufficient funds are available, and flags the transaction for capture (settlement) in a single invocation.<br/> 
6. AuthorizeAndCapture W/O token - This method proceeds with the card details and performs the capture operation in single invocation when the payment account data token is not available. <br/>
7. P2PE AuthorizeAndCapture- This method proceeds with the encrypted card details and performs the capture operation in single invocation when the payment account data token is not available. <br/>
8. Capture - The Capture operation is used to capture a single transaction for settlement after it has been successfully authorized by the Authorize operation. <br/>
9. Undo - The Undo operation is used to release card-holder funds by performing a void (Credit Card) or reversal (PIN Debit) on a previously authorized transaction that has not been captured (flagged) for settlement. <br/> 
10. Adjust - The Adjust operation is used to make adjustments to a previously authorized amount (incremental or reversal) prior to capture and settlement. <br/>
11. ReturnById - The ReturnById operation is used to perform a linked credit to a card-holder’s account from the merchant’s account based on a previously authorized and settled(Captured) transaction. <br/>
12. ReturnUnlinked - The ReturnUnlinked operation is used to perform an "unlinked", or standalone, credit to a card-holder’s account from the merchant’s account. <br/>
13. ReturnUnlinked W/O token - This method proceeds with the card details when payment account data token is not available.<br/>
14.P2PE ReturnUnlinked  - This method proceeds with the encrypted card details when payment account data token is not available.<br/>
15.QueryTransctionDetails-this method processes with transactionId or BatchID for the query of transaction.<br/>
16.CaptureAll - this method captures ,all the transaction made before.<br/>

Depending upon the type of transaction performed with request input data, response is generated from the velocity server which can be viewed on the Result page. <br/>
<h2>5.How to include static library into XcodeProject</h2><br/>
	1.Download the sdk from github<br/>
	2.Unzip and find the velocityLibrary folder .<br/>
	3.Drag and drop the folder into your existing XcodeProject.<br/>
	4.Include these headers where you are using the library variables.<br/>
	5.These files should be included<br/> 

		5.1 #import "Reachability.h"			//import this header
		5.2 #import "ErrorPaymentResponse.h"		//import this header
		5.3 #import "BankcardTransactionResponsePro.h"	//import this header
		5.4 #import "BancardCaptureResponse.h"		//import this header
		5.5 #import "VelocityResponse.h"		//import this header
		5.6 #import "VelocityPaymentTransaction.h"	//import this header
		5.7 #import "VelocityProcessor.h"               //import this header
		5.8 #import "QueryTransectionRequestModalClass.h"//import this header
		5.9 #import "CaptureAllArrayOfResponse.h"	//import this header
		5.10 #import "TransactionDetailModalClass.h"	//import this header

     
  


 

 
 



