<page>
    <actionBar title="Home" />
    <gridLayout>
        <label class="info">
            <formattedString>
                <span class="fas" text="&#xf135;" />
                <span text=" {message}" />
            </formattedString>
        </label>
    </gridLayout>
</page>

<script lang="ts">
    let message = ["lel"];

    import { Bluetooth } from '@nativescript-community/ble';
    import {Dialogs} from '@nativescript/core'

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
</script>