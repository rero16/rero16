- 👋 Hi, I’m @rero16
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...

<!---
rero16/rero16 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
==/用户脚本==


/**
* 前提：你可以提交作业至少两次
 *
* 使用方法：
* 首先点开作业，然后会跳出来随机做题选项，点击确认
* 然后会跳转到榜单页面，请手动点击"查看个人结果解析"
* 然后会跳出来是否解析答案，解析完成之后会提问是否跳转到光速做题页面
* 注意如果选择了光速做题页面则会在瞬间完成作业，如果不想这么显眼那就不要选择
* 手动回到做题页面也会自动填写正确答案
 */

const interval = setInterval（function() {
  如果 （！文档。querySelector（'.topic-item')) {
    返回;
  }

  如果（位置。呵呵。startsWith（'https://www.mosoteach.cn/web/index.php?c=interaction_quiz&m=person_quiz_result')) {
    如果 （！confirm（'检测到答案页面，是否开始解析？')) {
      返回;
    }

    const searchParams = new URL（location.href）.搜索参数;
    const title = [searchParams.get（'clazz_course_id'）， searchParams.get（'id'）].加入(' ');

    const problem = Array。from（document.querySelectorAll（'.topic-item'））.地图（（项) => {
      const subject = item。querySelector（'.t-subject'）.innerHTML;
      const 答案 = 项目。querySelector（'.answer-l .light'）.文本内容。修剪();
      if （/^[A-Z]$/g.测试（答案）)) {
         单项选择题
        const opt = item。querySelectorAll（'.opt'）[answer.字符代码At（0） - 65];
        返回 [
          主题，
          选择。querySelector（'.opt-content'）.innerHTML
        ];
      } 还 {
        返回 [ 主题，答案 ];
      }
    });

    本地存储。设置项(
      标题，
      JSON.字符串化（问题)
    );

    if （confirm（'答案已保存，是否开始光速做题？')) {
      位置。href = 位置。呵呵。replace（'m=person_quiz_result'， 'm=reply&bypass_confirm=1');
    }
  };

  如果（位置。呵呵。startsWith（'https://www.mosoteach.cn/web/index.php?c=interaction_quiz&m=reply')) {

    const searchParams = new URL（location.href）.搜索参数;
    const title = [searchParams.get（'clazz_course_id'）， searchParams.get（'id'）].加入(' ');

    const answer = localStorage。getItem（title);

    如果 （！答) {
      if （confirm（'未检测到答案，是否开始乱做？')) {
        文档。querySelectorAll（'.topic-item'）.forEach（（item) => {
          项。querySelector（'label'）.点击();
        });
      }
    } 还 {
      if （searchParams.get（'bypass_confirm'） || confirm（'检测到已有答案，是否开始填写？')) {
        const map = new Map（JSON.解析（答案));

        文档。querySelectorAll（'.topic-item'）.forEach（（item) => {
          const 答案 = 地图。get（item.querySelector（'.t-subject'）.innerHTML);
          如果 （！答) {
            alert（'找不到答案');
          }

          项。querySelectorAll（'label'）.forEach（（label) => {
            const 内容 = 标签。querySelector（'.option-content');
            if （content &content.innerHTML === 回答||标签。文本内容 === 答案) {
              标签。点击();
            }
          });
        });

        if （searchParams.get（'bypass_confirm'） || confirm（'是否提交？')) {
          文档。querySelector（'.con-bottom > button.el-button--primary'）.点击();
          设置超时(() => {
            文档。querySelector（'.el-message-box__btns button'）.点击();
          });
        }
      }
    }

  }

  清除间隔（间隔);
});
