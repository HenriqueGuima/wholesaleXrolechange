function send_email_to_admin_on_total_lifetime_value_reached( $order_id ) {
   // Get the order object
    $order = wc_get_order( $order_id );

    wholesalex();

    // Get the customer ID from the order
    $customer_id = $order->get_user_id();
	$should_switch = false;

    // Set the target total lifetime value
    $target_total_lifetime_value = 1000;

    // Calculate the total lifetime value for the customer
    $total_lifetime_value = wc_get_customer_total_spent( $customer_id );
	
	error_log('Customer ID: ' . $customer_id);
	error_log('Total Lifetime Value: ' . $total_lifetime_value);
	
	$user_data = get_userdata($customer_id);

	//GET USER NAME
	$user_name = $user_data->display_name;
		

    // If the customer's total lifetime value reached the desired amount
    if ( $total_lifetime_value >= $target_total_lifetime_value ) {
		$should_switch = true;
		
		if ($should_switch) {
									
			//GETS THE CURRENT ROLE ID
			$user_role_old = get_user_meta( $customer_id, '__wholesalex_role', true );
			$user_role_old_name = wholesalex()->get_role_name_by_role_id( $user_role_old );

			//error_log("User meta prev role: $user_role");
			error_log("User meta old role: $user_role_old_name");

			//CHANGE THE ROLE BY SPECIFYING THE ID
			wholesalex()->change_role( $customer_id, 'role_id', $user_role_old );
			$user_role_new = get_user_meta( $customer_id, '__wholesalex_role', true );
			$user_role_new_name = wholesalex()->get_role_name_by_role_id( $user_role_new );
			error_log("User meta new role: $user_role_new_name");

    	}
		
        $site_admin_email = 'name@example.pt';

        $to = $site_admin_email;
        $subject = "User reached $total_lifetime_value €!";
        $message = "USer $user_name reached $total_lifetime_value € and changed role from $user_role_old_name to $user_role_new_name";
        $headers = array('Content-Type: text/html; charset=UTF-8');
        wp_mail( $to, $subject, $message, $headers );
		
		//error_log('Customer ID: ' . $customer_id);
		//error_log('Total Lifetime Value: ' . $total_lifetime_value);
    }
}
add_action( 'woocommerce_order_status_completed', 'send_email_to_admin_on_total_lifetime_value_reached' );
