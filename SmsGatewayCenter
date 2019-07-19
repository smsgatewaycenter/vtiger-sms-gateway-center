<?php

	/* +**********************************************************************************
	 * The contents of this file are subject to the vtiger CRM Public License Version 1.0
	 * ("License"); You may not use this file except in compliance with the License
	 * The Original Code is:  vtiger CRM Open Source
	 * The Initial Developer of the Original Code is vtiger.
	 * Portions created by vtiger are Copyright (C) vtiger.
	 * All Rights Reserved.
	 * ********************************************************************************** */

	class SMSNotifier_SmsGatewayCenter_Provider implements SMSNotifier_ISMSProvider_Model {

		private $userName;
		private $password;
		private $parameters = array();

		const SERVICE_URI = 'https://www.smsgateway.center/SMSApi/rest/send';

		private static $REQUIRED_PARAMETERS = array('senderId');

		/**
		 * Function to get provider name
		 * @return <String> provider name
		 */
		public function getName() {
			return 'SmsGatewayCenter';
		}

		/**
		 * Function to get required parameters other than (userName, password)
		 * @return <array> required parameters list
		 */
		public function getRequiredParams() {
			return self::$REQUIRED_PARAMETERS;
		}

		/**
		 * Function to get service URL to use for a given type
		 * @param <String> $type like SEND, PING, QUERY
		 */
		public function getServiceURL($type = false) {
			if ($type) {
				switch (strtoupper($type)) {
					/* case self::SERVICE_AUTH:	return self::SERVICE_URI . '/http/auth';
					  case self::SERVICE_SEND:	return self::SERVICE_URI . '/http/sendmsg';
					  case self::SERVICE_QUERY:	return self::SERVICE_URI . '/http/querymsg'; */
					case self::SERVICE_AUTH: return self::SERVICE_URI;
					case self::SERVICE_SEND: return self::SERVICE_URI;
					case self::SERVICE_QUERY: return 'https://www.smsgateway.center/library/api/self/SMSDlrTrans/';
				}
			}
			return false;
		}

		/**
		 * Function to set authentication parameters
		 * @param <String> $userName
		 * @param <String> $password
		 */
		public function setAuthParameters($userName, $password) {
			$this->userName = $userName;
			$this->password = $password;
		}

		/**
		 * Function to set non-auth parameter.
		 * @param <String> $key
		 * @param <String> $value
		 */
		public function setParameter($key, $value) {
			$this->parameters[$key] = $value;
		}

		/**
		 * Function to get parameter value
		 * @param <String> $key
		 * @param <String> $defaultValue
		 * @return <String> value/$default value
		 */
		public function getParameter($key, $defaultValue = false) {
			if (isset($this->parameters[$key])) {
				return $this->parameters[$key];
			}
			return $defaultValue;
		}

		/**
		 * Function to prepare parameters
		 * @return <Array> parameters
		 */
		protected function prepareParameters() {
			$params = array('userId' => $this->userName, 'password' => $this->password);
			foreach (self::$REQUIRED_PARAMETERS as $key) {
				$params[$key] = $this->getParameter($key);
			}
			return $params;
		}

		/**
		 * Function to handle SMS Send operation
		 * @param <String> $message
		 * @param <Mixed> $toNumbers One or Array of numbers
		 */
		public function send($message, $toNumbers) {
			if (!is_array($toNumbers)) {
				$toNumbers = array($toNumbers);
			}
			$params = $this->prepareParameters();
			$params['sendMethod'] = 'simpleMsg';
			$params['msgType'] = 'dynamic';//system will detect unicode/text charset
			$params['msg'] = $message;
			$params['mobile'] = implode(',', $toNumbers);
			$params['format'] = 'json';
			$serviceURL = $this->getServiceURL(self::SERVICE_SEND);
			$httpClient = new Vtiger_Net_Client($serviceURL);
			$response = $httpClient->doGet($params);
			$responseLines = json_decode($response, true);
			$results = array();
			if ($responseLines->status != "success") {
				$result['error'] = true;
				$result['to'] = $params['mobile'];
				$result['statusmessage'] = $responseLines->reason; // Complete error message
				$result['status'] = self::MSG_STATUS_FAILED;
			} else {
				$result['id'] = $responseLines->transactionId;
				$result['to'] = $responseLines->mobile;
				$result['statusmessage'] = $responseLines->reason;
				$result['status'] = $responseLines->status == "success" ? self::MSG_STATUS_PROCESSING : self::MSG_STATUS_DISPATCHED;
			}
			$results[] = $result;
			return $results;
		}

		/**
		 * Function to get query for status using messgae id
		 * @param <Number> $messageId
		 */
		public function query($messageId) {
			$params = $this->prepareParameters();
			$params['transId'] = $messageId;
			$params['format'] = 'json';

			$serviceURL = $this->getServiceURL(self::SERVICE_QUERY);
			$httpClient = new Vtiger_Net_Client($serviceURL);
			$response = $httpClient->doGet($params);
			$response = json_decode($response, true);
			$result = array();
			$status = $response->status;
			// Capture the status code as message by default.
			$result['statusmessage'] = "Status: $status";
			if ($response['status'] != "success") {
				$result['error'] = true;
				$result['status'] = self::MSG_STATUS_FAILED;
			}
			$result['id'] = $messageId;
			$result['statusmessage'] = $response->reason;
			switch ($status) {
				case 'success':
					$result['status'] = self::MSG_STATUS_PROCESSING;
					break;
				case 'error':
					$result['status'] = self::MSG_STATUS_FAILED;
					$result['needlookup'] = 1;
					break;
				default:
					$result['error'] = true;
					$result['status'] = self::MSG_STATUS_FAILED;
					break;
			}
			return $result;
		}

	}
	
