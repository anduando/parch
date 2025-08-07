local Rayfield = loadstring(game:HttpGet("https://sirius.menu/rayfield"))()

local Window = Rayfield:CreateWindow({
   Name = "Remote Executor",
   LoadingTitle = "Loading...",
   ConfigurationSaving = {
      Enabled = false
   }
})

local Tab1 = Window:CreateTab("Remotes", 4483362458)
local Tab2 = Window:CreateTab("Send Args", 4483362458)

local function parseArgs(text)
   local args = {}
   for value in string.gmatch(text, "[^,]+") do
      local success, result = pcall(function()
         return loadstring("return " .. value)()
      end)
      table.insert(args, success and result or value)
   end
   return args
end

for _, remote in ipairs(game:GetDescendants()) do
   if remote:IsA("RemoteEvent") then
      Tab1:CreateButton({
         Name = remote:GetFullName(),
         Callback = function()
            pcall(function()
               remote:FireServer()
            end)
         end
      })

      local argInput = ""
      Tab2:CreateInput({
         Name = remote:GetFullName(),
         PlaceholderText = "arg1, arg2, 123, true",
         RemoveTextAfterFocusLost = false,
         Callback = function(input)
            argInput = input
         end
      })

      Tab2:CreateButton({
         Name = "Send to " .. remote.Name,
         Callback = function()
            pcall(function()
               local args = parseArgs(argInput)
               remote:FireServer(unpack(args))
            end)
         end
      })
   end
end
