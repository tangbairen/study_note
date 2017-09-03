有如下一个表单，

<form action="save.php" method="post">
    <input type="text" name="username[]" value="Jason" />
    <input type="text" name="username[]" value="Tom" />
    <input type="text" name="username[]" value="Goe" />

    <br />
    <label><input type="checkbox" name="book[]" value="Learn" />Lear</label>
    <label><input type="checkbox" name="book[]" checked="checked" value="PHP" />PHP</label>
    <label><input type="checkbox" name="book[]" value="Program" />Program</label>

    <br />
    <label><input type="radio" name="sex[]" checked="checked" value="Female" />Female</label>
    <label><input type="radio" name="sex[]" value="Male" />Male</label>
    <label><input type="radio" name="sex[]" value="Unknown" />Unknown</label>

    <br />
    <select name="order">
        <option value="first">first</option>
        <option value="second">second</option>
        <option value="third">third</option>
    </select>

    <p>&nbsp;</p>
    <button type="submit">提交</button>
</form>
save.php代码：

<?php
echo json_encode($_POST);


1 用serialize()方法提交

如果不用修改和验证数据，用$.post()提交最简单的方法是：

$('form').on('submit',function(e) {
    e.preventDefault();
    var data = $(this).serialize();

    $.post('save.php', data, function(result) {
        $('p').text(result);
    });
});
返回数据：

{"username":["Jason","Tom","Goe"],"book":["PHP"],"sex":["Female"],"order":"first"}