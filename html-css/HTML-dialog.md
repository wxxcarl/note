# `<dialog>`
## 属性
- ### open
    直接显示对话框，表示这个对话框可以进行互动
- ### returnValue
    用户获取对话框的值

## 方法
- ### close()
    关闭对话框。 可选传入类型为{domxref("DOMString")}}的参数，用来更新对话框的`returnValue`。
- ### show()
    非模式化的显示这个对话框，不会覆盖其他DOM，可选传入类型为 `Element` 或者 `MouseEvent` 的参数。
- ### showModal()
    模式化的显示这个对话框， 并且将会至于所有其他对话框的顶层，可选传入类型为 `Element` 或者 `MouseEvent` 的参数。用这种方法打开的dialog，可以按`ESC`关闭。


# 样式
    /* 默认样式 */
    dialog {
        display: block;
        position: absolute;
        left: 0px;
        right: 0px;
        width: -webkit-fit-content;
        height: -webkit-fit-content;
        color: black;
        margin: auto;
        border-width: initial;
        border-style: solid;
        border-color: initial;
        border-image: initial;
        padding: 1em;
        background: white;
    }
    dialog::backdrop {
        position: fixed;
        top: 0px;
        right: 0px;
        bottom: 0px;
        left: 0px;
        background: rgba(0, 0, 0, 0.1);
    }

    /* 重置背景色 */
    dialog::backdrop{
        background-color: rgba(0, 0, 0, 0.6)
    }

# Example
    <dialog id="dialogEx">
      <h1>我是一级标题</h1>
      <div>我是对话窗口，你已经打开了我！</div>
      <button id="close_dialog">关闭</button>
    </dialog>
    <button id="open_dialog">打开</button>

    <script>
      var dialog = window.dialogEx,
      openDialog = window.open_dialog,
      closeDialog = window.close_dialog;
      openDialog.onclick = function(){
          dialog.showModal(); // 或者show(),这是有区别的
      }
      closeDialog.onclick = function(){
          dialog.close('知道了');
          console.log(dialog.returnValue)
      }
    </script>`


<style>
    .page-header {
        display: none;
    }
</style>