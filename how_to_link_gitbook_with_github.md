# how to link gitbook with github
如何将gitbook和github关联？
 需要先在github上建立一个repository,例如repo.
 然后在gitbook上建立一本book,例如note.
 再在gitbook中选择setting,在左侧选择github，然后选择repo，如果两者的登录用户相同，系统就会自动将2者关联.
 比如我现在在gitbook中的table of contents中添加一个新文件，命名为how_to_link_gitbook_with_github.md
 保存后就会在github的repo中多出一个同名文件，文件内容就是gitbook文件的内容。
 如果顺序反了，先写book，再建repo，虽然可以按帮助的提示授权，但会出问题，我是把book和repo全都删除了重做才关联好的。
 
 如果文件不是在gitbook中创建的，那么它虽然出现在files tree中，但不会出现在table of contents中，也不会出现在最终的电子书(如pdf)中。
 
  
 编辑器还是gitbook的功能更强，所以应该在gitbook里编辑。需要当心，如果同时在github和gitbook中编辑一个文件，很可能覆盖其中一个作的修改。
