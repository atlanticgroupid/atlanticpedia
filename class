<?php
class AtlanticPedia
{
    private $base_url;
    private $api_key;
    
    public function __construct($x) {
        if(isset($x['base']) && isset($x['key'])) {
            $this->base_url = $x['base'];
            $this->api_key = $x['key'];
        }
    }
    
    public function profile() {
        $try = $this->connect('/profile');
        return [
            'result' => $try['result'],
            'data' => $try['result'] == false ? '' : $try['data'],
            'message' => $try['result'] == false ? $try['data'] : 'Profile data from Atlantic Pedia was successfully obtained.'
        ];
    }
    
    public function order($x,$data) {
        if(in_array($x,['pulsa','games','sosmed']) && is_array($data)) {
            $data['action'] = 'order';
            if($x == 'pulsa') $try = $this->connect('/pulsa',$data);
            if($x == 'games') $try = $this->connect('/game',$data);
            if($x == 'sosmed') $try = $this->connect('/sosmed',$data);
            return [
                'result' => $try['result'],
                'data' => $try['result'] == false ? '' : $try['data'],
                'message' => $try['result'] == false ? $try['data'] : 'Successfully placed orders at Atlantic Pedia.'
            ];
        } else {
            return ['result' => false,'data' => null,'message' => 'Invalid Request!'];
        }
    }
    
    public function status($x,$id) {
        if(in_array($x,['pulsa','games','games-server','sosmed']) && !empty($id)) {
            if($x == 'pulsa') $try = $this->connect('/pulsa',['action' => 'status','trxid' => $id]);
            if($x == 'games') $try = $this->connect('/game',['action' => 'status','trxid' => $id]);
            if($x == 'games-server') $try = $this->connect('/game',['action' => 'server','server' => $id]);
            if($x == 'sosmed') $try = $this->connect('/sosmed',['action' => 'status','trxid' => $id]);
            return [
                'result' => $try['result'],
                'data' => $try['result'] == false ? '' : $try['data'],
                'message' => $try['result'] == false ? $try['data'] : 'Status data from Atlantic Pedia successfully obtained.'
            ];
        } else {
            return ['result' => false,'data' => null,'message' => 'Invalid Request!'];
        }
    }
    
    public function services($x) {
        if(in_array($x,['pulsa','games','sosmed'])) {
            if($x == 'pulsa') $try = $this->connect('/pulsa',['action' => 'services']);
            if($x == 'games') $try = $this->connect('/game',['action' => 'services']);
            if($x == 'sosmed') $try = $this->connect('/sosmed',['action' => 'services']);
            return [
                'result' => $try['result'],
                'data' => $try['result'] == false ? '' : $try['data'],
                'message' => $try['result'] == false ? $try['data'] : 'Service data from Atlantic Pedia was successfully obtained.'
            ];
        } else {
            return ['result' => false,'data' => null,'message' => 'Invalid Request!'];
        }
    }
    
    public function deposit($x,$data = '') {
        if(in_array($x,['request','check','confirm','cancel','method'])) {
            if($x == 'request' && is_array($data)) {
                $try = $this->connect('/deposit',array_merge(['action' => 'create'],$data));
                return [
                    'result' => $try['result'],
                    'data' => $try['result'] == false ? '' : $try['data'],
                    'message' => $try['result'] == false ? $try['data'] : 'Deposit Request successfully made.'
                ];
            } if($x == 'check') {
                $arr = !empty($data) ? ['action' => 'check','id' => $data] : ['action' => 'check'];
                $try = $this->connect('/deposit',$arr);
                return [
                    'result' => $try['result'],
                    'data' => $try['result'] == false ? '' : $try['data'],
                    'message' => $try['result'] == false ? $try['data'] : 'Deposit data checked successfully.'
                ];
            } if($x == 'confirm' && !empty($data)) {
                $try = $this->connect('/deposit',['action' => 'accept','id' => $data]);
                return ['result' => $try['result'],'data' => $try['data'],'message' => $try['data']];
            } if($x == 'cancel' && !empty($data)) {
                $try = $this->connect('/deposit',['action' => 'cancel','id' => $data]);
                return ['result' => $try['result'],'data' => $try['data'],'message' => $try['data']];
            } if($x == 'method') {
                $try = $this->connect('/deposit',['action' => 'method']);
                return [
                    'result' => $try['result'],
                    'data' => $try['result'] == false ? '' : $try['data'],
                    'message' => $try['result'] == false ? $try['data'] : 'Deposit method successfully obtained.'
                ];
            } else {
                return ['result' => false,'data' => null,'message' => 'Invalid Request!'];
            }
        } else {
            return ['result' => false,'data' => null,'message' => 'Invalid Request!'];
        }
    }

    # END POINT CONNECTION #

    public function connect($file,$data = []) {
        $ch = curl_init();
        curl_setopt($ch, CURLOPT_URL, $this->base_url.$file);
        curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
        curl_setopt($ch, CURLOPT_FOLLOWLOCATION, 1);
        curl_setopt($ch, CURLOPT_HTTPHEADER, ['Content-Type: multipart/form-data;']);
        curl_setopt($ch, CURLOPT_POST, 1);
        $data['key'] = $this->api_key;
        curl_setopt($ch, CURLOPT_POSTFIELDS, $data);
        $chresult = curl_exec($ch);
        return json_decode($chresult, true);
    }
}

$APedia = new AtlanticPedia([
    'base' => 'https://atlantic-pedia.co.id/api', // BASE URL API
    'key' => '', // API KEY ANDA
]);


// PROFILE
// print json_encode($APedia->profile(), JSON_PRETTY_PRINT);

// PULSA DLL
// print json_encode($APedia->order('pulsa',['service' => '','target' => '']), JSON_PRETTY_PRINT); // Buat Order
// print json_encode($APedia->status('pulsa','ID Transaksi'), JSON_PRETTY_PRINT); // Status Transaksi
// print json_encode($APedia->services('pulsa'), JSON_PRETTY_PRINT); // Data Layanan

// GAMES
// print json_encode($APedia->order('games',['service' => '','target' => '']), JSON_PRETTY_PRINT); // Buat Order
// print json_encode($APedia->status('games','ID Transaksi'), JSON_PRETTY_PRINT); // Status Transaksi
// print json_encode($APedia->status('games-server','ID Server | SRV-1'), JSON_PRETTY_PRINT); // Status Server
// print json_encode($APedia->services('games'), JSON_PRETTY_PRINT); // Data Layanan

// SOSIAL MEDIA
// print json_encode($APedia->order('sosmed',['service' => '','target' => '','quantity' => '']), JSON_PRETTY_PRINT); // Buat Order
// print json_encode($APedia->status('sosmed','ID Transaksi'), JSON_PRETTY_PRINT); // Status Transaksi
// print json_encode($APedia->services('sosmed'), JSON_PRETTY_PRINT); // Data Layanan

// DEPOSIT
// print json_encode($APedia->deposit('request',['payment' => '','method' => '','quantity' => '','sender' => '']), JSON_PRETTY_PRINT); // Buat Deposit
// print json_encode($APedia->deposit('check','ID Transaksi'), JSON_PRETTY_PRINT); // Cek Deposit
// print json_encode($APedia->deposit('confirm','ID Transaksi'), JSON_PRETTY_PRINT); // Confirm Deposit
// print json_encode($APedia->deposit('cancel','ID Transaksi'), JSON_PRETTY_PRINT); // Cancel Deposit
// print json_encode($APedia->deposit('method'), JSON_PRETTY_PRINT); // Metode Deposit
