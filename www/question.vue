<template>
    <div class="app">
      <textarea v-model="myText"></textarea>
			<p>文章は「{{ myText }}」です。</p>
			<p>文字数は、{{　myText.length　}}文字です。</p>
      
      <div class="center"> Firebaseストレージ </div>
      <section style="margin: 10px;">
          <input type="file" name="photo" @change="fileChange" accept="image/*" />
          <button @click="upload">アップロード</button>
          <div v-if="photo_url">
              <div class="center">
                  <img :src="photo_url" width="80%" />
              </div>
          </div>
      </section>
    </div>
</template>

<script>
    new Vue({
    el:'#app',
    data:{
      myText:'こんにちは',
      // タグの追加
      dataArray:[],
          addItem:'',
      //写真のアップロード
      photo: null,
      photo_url: null
    },
    methods:{
      // タグの追加
      addList:function(){
        this.dataArray.push(this.addItem);
        this.addItem='';
      },
      removeList:function(){
        var lastIdx = this.dataArray.length - 1;
        this.dataArray.splice(lastIdx, 1);
      },
    },
    // ファイルを指定した時の処理
    fileChange: function(e) {
      this.photo = e.target.files[0];
    },
    // 画像アップロード処理
    upload: function() {
        var me = this;
        // ストレージオブジェクト作成
        var storageRef = firebase.storage().ref();
        // ファイルのパスを設定
        var mountainsRef = storageRef.child(`images/${this.photo.name}`);
        // ファイルを適用してファイルアップロード開始
        var uploadTask = mountainsRef.put(this.photo);
        // ステータスを監視
        uploadTask.on('state_changed', function(snapshot){
        }, function(err) {
            console.log(err);
        }, function() {
            // アップロード完了したら画像のURLを取得
            me.photo_url = uploadTask.snapshot.downloadURL;
        })
    }
  })
  </script>