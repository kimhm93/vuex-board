<template>
    <ul class="comments">
        <li v-for="comment in comments" :key="comment.id">
            <!-- <div class="comment-item">
                <strong>{{ comment.user.name }}</strong><span>{{ comment.createdAt }}</span>
                <p>{{ comment.contents }}</p>
            </div> -->
            <comment-item :comment="comment" @edit="onEdit" @delete="onDelete"/>
        </li>
        <li v-if="comments.length <= 0">
            입력된 댓글이 없습니다.
        </li>
    </ul>
</template>
<script>
import CommentItem from '@/components/CommentItem';
import { mapActions } from 'vuex'; 

export default{
    name: 'CommentList',
    components: {
        CommentItem,
    },
    props: {
        comments : {
            type : Array,
            default() {
                return  [];
            }
        }
    },
    methods: {
        onEdit(payload) {
          const {id, comment} =  payload;

          this.editComment({ commentId : id, comment})
            .then(res => {
                alert('댓글 수정이 완료되었습니다.');
            })
            .catch(err => {
                if( err.response.status === 401 ){
                    alert('로그인이 필요합니다.');
                    this.$router.push({ name: 'Signin' });
                } else{
                    alert(err.response.data.msg);
                }
            })
        },
        onDelete(commentId) {
            this.deleteComment(commentId)
            .then(() => {
                alert('댓글이 삭제되었습니다.');
            })
            .catch(err => {
                if(err.response.status === 401){
                    alert('로그인이 필요합니다.');
                    this.$router.push({ name : 'Signin' });
                } else{
                    alert(err.response.data.msg);
                }
            })
        },
        ...mapActions([
            'editComment',
            'deleteComment'
        ])
    }

}
</script>