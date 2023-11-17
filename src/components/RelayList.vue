<template>
  <div>
    <ul>
      <li v-for="relay in relays" :key="relay.id">
        Device ID: {{ relay.deviceId }}, 
        Target State: <input type="checkbox" :checked="relay.targetState" @change="updateTargetState(relay.id, $event)">
        Current State: {{ relay.currentState }}
      </li>
    </ul>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      relays: []
    }
  },
  mounted() {
    this.fetchRelays();
  },
  methods: {
    fetchRelays() {
      axios.get(`${process.env.VUE_APP_API_URL}/temp/relays`)
        .then(response => {
          this.relays = response.data;
        })
        .catch(error => {
          console.error('Error fetching relays:', error);
        });
    },
    updateTargetState(id, event) {
      const newTargetState = event.target.checked;
      axios.put(`${process.env.VUE_APP_API_URL}/temp/relays/${id}`, { targetState: newTargetState })
        .then(() => {
          this.fetchRelays();
        })
        .catch(error => {
          console.error('Error updating target state:', error);
        });
    }
  }
}
</script>