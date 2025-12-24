# MAIN 
resource "aws_iam_user" "users" {
    for_each = { for user in local.users : user.first_name => user }  


    name = lower("${substr(each.value.first_name, 0, 1)}${each.value.last_name}")
    path="/users/"

    tags={
        "DisplayName" = "${each.value.first_name} ${each.value.last_name}"
    "Department"  = each.value.department
    "JobTitle"    = each.value.job_title
    }
}

resource "aws_iam_user_login_profile" "users" {
  for_each =  aws_iam_user.users
  user=each.value.name
  password_reset_required = true


  lifecycle {
    ignore_changes = [
      password_length,
      password_reset_required,
    ]
  }

}



# OUTPUT
output "account_id" {
  value = data.aws_caller_identity.name
}

output "user_names" {
  value = [ for user in local.users: "${user.first_name} ${user.last_name}"]
}

output "user_passwords" {
  value = {
    for user,profile in aws_iam_user_login_profile.users:
    user=>"password created reset please"

  }
  sensitive = true
}


# GROUP
#Create IAM Groups
resource "aws_iam_group" "education" {
  name = "Education"
  path = "/groups/"
}

resource "aws_iam_group" "managers" {
  name = "Managers"
  path = "/groups/"
}

resource "aws_iam_group" "engineers" {
  name = "Engineers"
  path = "/groups/"
}

#Add users to the Education group
resource "aws_iam_group_membership" "education_members" {
  name  = "education-group-membership"
  group = aws_iam_group.education.name

  users = [
    for user in aws_iam_user.users : user.name if user.tags.Department == "Education"
  ]
}

#Add users to the Managers group
resource "aws_iam_group_membership" "managers_members" {
  name  = "managers-group-membership"
  group = aws_iam_group.managers.name

  users = [
    for user in aws_iam_user.users : user.name if contains(keys(user.tags), "JobTitle") && can(regex("Manager|CEO", user.tags.JobTitle))
  ]
}

#Add users to the Engineers group
resource "aws_iam_group_membership" "engineers_members" {
  name  = "engineers-group-membership"
  group = aws_iam_group.engineers.name

  users = [
    for user in aws_iam_user.users : user.name if user.tags.Department == "Engineering" # Note: No users match this in the current CSV
  ]
}



