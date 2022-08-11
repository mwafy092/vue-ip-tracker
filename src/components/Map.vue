<template>
    <div style="height: calc(100vh - 250px); width: 100vw">
        <l-map
            v-model="zoom"
            v-model:zoom="zoom"
            :center="[this.location.lat || 0, this.location.lng || 0]"
            @move="log('move')"
        >
            <l-tile-layer
                url="https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png"
            ></l-tile-layer>
            <l-control-layers />

            <l-marker
                :lat-lng="[this.location.lat || 0, this.location.lng || 0]"
            >
                <l-icon :icon-url="iconUrl" :icon-size="iconSize" />
            </l-marker>
        </l-map>
    </div>
</template>
<script>
import {
    LMap,
    LIcon,
    LTileLayer,
    LMarker,
    LControlLayers,
    LTooltip,
    LPopup,
    LPolyline,
    LPolygon,
    LRectangle,
} from '@vue-leaflet/vue-leaflet';
import 'leaflet/dist/leaflet.css';

export default {
    name: 'Map',
    props: ['lat', 'lng'],
    components: {
        LMap,
        LIcon,
        LTileLayer,
        LMarker,
        LControlLayers,
        LTooltip,
        LPopup,
        LPolyline,
        LPolygon,
        LRectangle,
    },
    data() {
        return {
            zoom: 5,
            iconWidth: 40,
            iconHeight: 50,
        };
    },
    computed: {
        location() {
            return { lat: this.lat, lng: this.lng };
        },
        iconUrl() {
            return 'icon-location.svg';
        },
        iconSize() {
            return [this.iconWidth, this.iconHeight];
        },
    },
    methods: {
        log(a) {
            console.log(a);
        },
        changeIcon() {
            this.iconWidth += 2;
            if (this.iconWidth > this.iconHeight) {
                this.iconWidth = Math.floor(this.iconHeight / 2);
            }
        },
    },
};
</script>
