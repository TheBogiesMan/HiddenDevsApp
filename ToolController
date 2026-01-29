local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Players = game:GetService("Players")

local Observers = require(ReplicatedStorage.Packages.Observers)
local Debounces = require(ReplicatedStorage.Packages.Debounces)
local LocalPlayer = Players.LocalPlayer

local ToolController = {
	_classes = {}
}

local function GetToolOwner(Model)
	local Parent = Model.Parent
	if Parent:IsA("Backpack") then 
		return Parent.Parent
	elseif Parent:FindFirstChildOfClass("Humanoid") then 
		return Players:GetPlayerFromCharacter(Parent)
	end
end

function ToolController:SetupTool(Tool)
	local Type = Tool:GetAttribute("Type")
	
	local Class = require(script[Type]).new(Tool) -- fetch the data from within the class
	self._classes[Tool] = Class

	Tool.Activated:Connect(function()
		Class:OnActivated(LocalPlayer, Tool) -- do the debounce logic in there also rename function to "OnActivated"
	end)
end

function ToolController:RemoveTool(Tool)
	local Class = self._classes[Tool]
	if Class then
		Class:Destroy()
        --[[
        this method should do something like this
        self.Trove:Destroy() make sure to add a trove
        for k in self do 
            self[k] = nil
        end
        setmetatable(self, nil)
        ]]
	end
end

function ToolController:Start()
	Observers.observeTag("Tool", function(Tool)
		if GetToolOwner(Tool) ~= LocalPlayer then return end 

		self:SetupTool(Tool)

		return function()
			self:RemoveTool(Tool)
		end
	end)
end

return ToolController
