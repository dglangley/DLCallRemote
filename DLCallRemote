<?php
    // Simple function that uses cURL, minimizing repetitive typing of the same cURL options and commands

    $USER_AGENT = 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/74.0.3729.157 Safari/537.36';
    $DEV_ENV = false;
    $FOLLOW_LOCATION = false;
    function call_remote($base,$params,&$cookiefile,&$cookiejarfile,$getpost='GET',$global_ch=false, $timeout = 0, $header = false) {
        global $USER_AGENT;

        if ($global_ch) { $ch = $global_ch; }
        else { $ch = curl_init($base); }

        curl_setopt($ch, CURLOPT_REFERER, $base);
        curl_setopt($ch, CURLOPT_CONNECTTIMEOUT, $timeout);//mostly for T-E
        curl_setopt($ch, CURLOPT_TIMEOUT, $timeout);//mostly for T-E
        curl_setopt($ch, CURLOPT_HTTPAUTH, CURLAUTH_ANY);
        curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
        if ($header) {
            curl_setopt($ch, CURLOPT_HTTPHEADER, $header);
        }
        curl_setopt($ch, CURLOPT_FOLLOWLOCATION, $GLOBALS['FOLLOW_LOCATION']);
        curl_setopt($ch, CURLOPT_VERBOSE, true);
        if ($cookiefile AND $cookiejarfile) {
            curl_setopt($ch, CURLOPT_COOKIESESSION, true);
            curl_setopt($ch, CURLOPT_COOKIEFILE, $cookiefile);
            curl_setopt($ch, CURLOPT_COOKIEJAR, $cookiejarfile);
        } else {
            curl_setopt($ch, CURLOPT_COOKIESESSION, false);
        }
        //we want this only for development environment to avoid MitM attack?
        if ($GLOBALS['DEV_ENV']) {
            curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, false);
        }

        if (isset($_SERVER['HTTP_USER_AGENT']) AND $_SERVER['HTTP_USER_AGENT']) {
            curl_setopt($ch, CURLOPT_USERAGENT, $_SERVER['HTTP_USER_AGENT']);
        } else {
            curl_setopt($ch, CURLOPT_USERAGENT, $GLOBALS['USER_AGENT']);
        }
        if ($getpost=='GET') {
            curl_setopt($ch, CURLOPT_POST, false);
            curl_setopt($ch, CURLOPT_HTTPGET, true);
            curl_setopt($ch, CURLOPT_URL, $base.$params);
        } else {
            curl_setopt($ch, CURLOPT_POST, true);
            curl_setopt($ch, CURLOPT_HTTPGET, false);
            curl_setopt($ch, CURLOPT_POSTFIELDS, $params);
            curl_setopt($ch, CURLOPT_URL, $base);
        }

        $res = curl_exec($ch);

        // if we don't have a global connection we want left open, close the connection upon completion of this script
        if (! $global_ch) { curl_close($ch); }

        return ($res);
    }
?>
