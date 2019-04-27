<?php
namespace Platform\Yunji;

class Yunji
{
    public static $inc = null;
    public static $shop_name = "";
    public $url = "https://op.yunjiglobal.com/opgateway/api/openapi";
    private $appKey;
    private $appSecret;
    private $customerId;
    public $body = '';
    private $format = 'json';
    private $timestamp = '';
    private $v = '2.0';
    private $sign_method = 'md5';
    private $param_arr = array();

    public function __construct($appKey = '', $appSecret = '', $customerId = '')
    {
        $this->appKey = $appKey;
        $this->appSecret = $appSecret;
        $this->customerId = $customerId;
    }

    public function __set($k, $v)
    {
        $this->param_arr[$k] = $v;
    }

    //生成签名
    public function createsign()
    {
        //计算加密参数时用到的参数
        $this->param_arr['timestamp'] = date('Y-m-d H:i:s');
        $this->param_arr['format'] = $this->format;
        $this->param_arr['app_key'] = $this->appKey;
        $this->param_arr['v'] = $this->v;
        $this->param_arr['sign_method'] = $this->sign_method;
        $this->param_arr['customer_id'] = $this->customerId;

        $sign_arr = $this->param_arr;
        ksort($sign_arr);
        $sign_str = '';
        foreach ($sign_arr as $key => $val) $sign_str .= $key . $val;
        return $sign_str;
    }

    public function request()
    {
        $sign_str = $this->createsign();
        $sign = $this->appSecret . $sign_str . $this->body . $this->appSecret;
        $sign = md5($sign);
        $this->param_arr['sign'] = $sign;
        $response = $this->vpost($this->url, $this->param_arr);
        $this->param_arr = array();
        return $response;
    }

    public function vpost($url, $data)
    {
        $ch = curl_init();
        $url = $url . '?' . http_build_query($data);
        curl_setopt($ch, CURLOPT_URL, $url);
        curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
        curl_setopt($ch, CURLOPT_HTTPHEADER, ["Content-type: application/json;charset='utf-8'"]);
        curl_setopt($ch, CURLOPT_POST, 1);
        curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, false);
        curl_setopt($ch, CURLOPT_POSTFIELDS, $this->body);
        $response = curl_exec($ch);
        $response = json_decode($response, true);
        if (curl_errno($ch)) {
            echo 'Errno' . curl_error($ch);
        }
        curl_close($ch);
        return $response;
    }
}

