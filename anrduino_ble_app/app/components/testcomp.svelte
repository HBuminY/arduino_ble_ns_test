<script lang="ts">
    let message: string = "amucuk uygulaması"

    // require the plugin
    import { Bluetooth } from '@nativescript-community/ble';
    import { check as checkPermission, request as requestPermission } from '@nativescript-community/perms';
    import {Dialogs} from '@nativescript/core'
    var bluetooth = new Bluetooth();


    let detectedDevices = [];
    function deviceDetected(device){
        detectedDevices.push(device)
    }

    function scanDevices(){
        bluetooth.startScanning({
            seconds: 4,
            onDiscovered: function (peripheral) {
                deviceDetected(peripheral)
            }
        }).then(function() {
            Dialogs.alert("scanning complete");
        }, function (err) {
            Dialogs.alert("error while scanning: " + err);
        });
    }
    
    function requestScanPerm(){
        requestPermission('bluetoothScan', { type: 'always' }).then(response => {
            if(response[1]){
                checkIfBLenabled()
            }else{
                requestScanPerm();
            }
        })
    }

    function requestLocation(){
        bluetooth.requestLocationPermission().then((granted)=>{
                if(granted){
                    scanDevices();
                }else{
                    Dialogs.confirm({
                        title: 'Konum İzni Kapalı',
                        message: "Lütfen konumuu açın",
                        okButtonText: 'tamam'
                    }).then((result) => {
                        requestLocation();
                    })
                }
            }
        );
    }

    function checkIfBLenabled(){
        bluetooth.isBluetoothEnabled().then(
            function(enabled) {
                if(enabled){
                    requestLocation();
                }else{
                    Dialogs.confirm({
                        title: 'Bluetooth Kapalı',
                        message: "Lütfen Bluetooth'u açın",
                        okButtonText: 'tamam'
                    }).then((result) => {
                        checkIfBLenabled();
                    })
                }
            }
        );
    }
    
    requestScanPerm();