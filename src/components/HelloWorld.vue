<template>
  <div class="container-fluid">
    <h2>Vue Opentok Sample</h2>
    <div class="col-lg-4" style="margin-top: 40px;">
      <form @submit.prevent="connectOpentok">
        <div class="form-group">
          <label for="" class="h5">API key</label>
          <input type="text" class="form-control" required 
            v-model="form.api_key"
          />
        </div>
        <div class="form-group">
          <label for="" class="h5">Session ID</label>
          <input type="text" class="form-control" required 
            v-model="form.session_id"
          />
        </div>
        <div class="form-group">
          <label for="" class="h5">Session token</label>
          <input type="text" class="form-control" required 
            v-model="form.session_token"
          />
        </div>
        <button class="btn btn-primary">Conectar</button>
      </form>
    </div>
    
    <div id="publisher">
      <p>publisher</p>
    </div>
    <div id="subscriber">
      <p>subscriber</p>
    </div>
  </div>
</template>

<script>
import OT from '@opentok/client';
export default {
  data() {
    return {
      form: {
        api_key: "46877804",
        session_id: '',
        session_token: '',
      },
      session: null,
      streams: []
    }
  },
  beforeDestroy() {
    /* var apiKey = this.form.api_key
    var sessionId = this.form.session_id
    var session = OT.initSession(apiKey, sessionId)

    session.disconnect() */
  },
  methods: {
    connectOpentok() {
      var apiKey = this.form.api_key
      var sessionId = this.form.session_id
      var token = this.form.session_token
      //var targetElem = document.getElementById("subscriber");

      var session = OT.initSession(apiKey, sessionId)
      
      // Subscribe to a newly created stream
      session.on('streamCreated', function(event) {
        console.log('streamCreated')
        session.subscribe(event.stream, 'subscriber', {
          insertMode: 'append',
          width: '100%',
          height: '100%'
        });
        session.unpublish(publisher_1);

        var publisher_2 = OT.initPublisher('publisher', {
          insertMode: 'append',
          width: '100%',
          height: '100%'
        });
        session.publish(publisher_2);
      });


      // Disconnect
      session.on('connectionDestroyed', function() {
        console.log('connectionDestroyed')
        //session.unpublish(publisher)
        session.disconnect()
      });


      // Create a publisher
      var publisher_1 = OT.initPublisher('publisher', {
        insertMode: 'append',
        width: '100%',
        height: '100%'
      });

      // Connect to the session
      session.connect(token, function(error) {
        // If the connection is successful, initialize a publisher and publish to the session
        if (error) {
          console.log(error);
        } else {
          console.log('connected')
          session.publish(publisher_1);
        }
      });      
    }
  }
  
}
</script>

<style scoped>
#publisher {
  float: left;
  width: 600px;
  height: 400px;
  border: 1px solid;
}
#subscriber {
  float: right;
  width: 600px;
  height: 400px;
  border: 1px solid;
}
</style>