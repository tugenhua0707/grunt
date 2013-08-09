grunt
=====

使用gruntjs构建web程序
  
    下面来介绍下如何用grunt合并，压缩，处理依赖的js文件。
    大概步骤有如下：
      1. 新建文件夹相对应的项目 比如文件名叫：gruntJs
      2. 新建文件package.json。
      3. 新建文件Gruntfile.js。
      4. 命令行执行grunt任务。
    
    一： 新建文件名为：gruntJs 该根目录下有src文件夹 里面放了n个js文件要构建的，还有个叫dest文件夹(名字都可以自取)，这个
         文件是存储编译后的js文件。
         
    二： 新建package.json 
         
         package.json放在根目录下，它包含了该项目的一些元信息，如项目名称、描述、版本号，依赖包等。
         它应该和源码一样被提交到svn或git。 现在的内容如下：
          {
            "name": "gruntJs",
            "version": "1.0.0",
            "devDependencies": {
              "grunt": "~0.4.0",
              "grunt-contrib-jshint": "~0.1.1",
              "grunt-contrib-uglify": "~0.1.2",
              "grunt-contrib-concat": "~0.1.1"  
            }
          }
        配置时候 版本一定要对应，否则打包会报错。上面是正确的。
           
           grunt-contrib-jshint js语法检查 
           grunt-contrib-uglify 压缩，采用UglifyJS
           grunt-contrib-concat 合并文件
           
        此时： 打开命令行工具进入到项目根目录，如（gruntJs）敲如下命令: npm install 后 如果一切正常的话 那么
         再查看根目录，发现多了个node_modules目录，包含了5个子目录 grunt-cmd-transport  grunt-contrib-clean
         grunt-contrib-jshint grunt-contrib-uglify grunt。
         
    三：新建文件Gruntfile.js 
        1。 wrapper函数，结构如下，这是Node.js的典型写法，使用exports公开API 
            module.exports = function(grunt) {
                // Do grunt-related things in here
            };
        2.  项目和任务配置。
        3.  载入grunt插件和任务。
        4.  定制执行任务。
     
  该示例完成以下任务：
    合并src下的文件（grunt.js/bb.js）为domop.js
    压缩domop.js为domop.min.js
    这两个文件都放在dest目录下
  最终的Gruntfile.js如下:
      module.exports = function(grunt) {
        // 配置
        grunt.initConfig({
            pkg : grunt.file.readJSON('package.json'),
          	transport: {  //处理依赖
                    dialog: {
                        files : [
                            {
                                src : 'src/*.js',
                                dest : 'build/'
                            }
                        ]
                    }
                },
              concat: {
                  domop: {
                      src: ['src/grunt.js', 'src/bb.js'],
                      dest: 'dest/domop.js'
                  }
              },
              uglify: {
                  build: {
                      src : 'dest/domop.js',
                      dest : 'dest/domop-min.js'
                  }
              }
          });
          // 载入concat和uglify插件，分别对于合并和压缩
      	  grunt.loadNpmTasks('grunt-cmd-transport');
          grunt.loadNpmTasks('grunt-contrib-concat');
          grunt.loadNpmTasks('grunt-contrib-uglify');
          
          // 注册任务
          grunt.registerTask('default', ['transport','concat','uglify']);
      }; 
  四、执行grunt任务。
      
      打开命令行，进入到项目根目录，敲 grunt  如果一切都成功的话 那么 
      
      看出成功的合并和压缩并生成了dest目录及期望的文件，这时的项目目录下多了dest

    
          
          
          
